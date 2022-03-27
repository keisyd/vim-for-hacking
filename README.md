# Tomnomnom - Linux Termial Tools For Bug Bounty

This is an interesting Tomnomnom video that put me into this hacking world, so I decided to give it a full tutorial folder. 

We're about to see a bunch of Vim tricks and usage of Tomnomnom tools.

# Sub-domain enumeration with assetfinder

It wasn't clear at the beggining of the video, but a lot of downloading his tools had to happen for his steps to be repeated.

For this one we're using [assetfinder](https://github.com/tomnomnom/assetfinder) that is a really interesting tool to find domains and subdomains. Go there to download and place this binary into you /usr/local/bin. I'll be wating...

> How to? 
> - Go to the assetifinder repository
> - Go to [Releases](https://github.com/tomnomnom/assetfinder/releases) download the compressed file for you os.
> - Run `tar -C /usr/local/bin -xzf <the downloaded release>` 
> - Enjoy.

After a while we're back to documentation and all we're going to do here is to use this tool in a fairly simple way:

`assetfinder --subs-only uber.com`

And this ruge output is going to be the inspiration to start using bash like a crazy, so, let's go for it.

Almost everything outputed is going to lay here in this same repo folder! Enjoy.

The first nice thing is to use th arrow comand to output everything from the previous command into a file:

`assetfinder --subs-only uber.com > domains`

So with `ls` you should see the file (the terminal output is going to be domains) and with `cat domains` you'll have the same first output.

## Pre-filtering to find http servers

Not all subdomains will have a http server, so the prefilter comes handy and let's go for more Tomnomnom tools.

The one now is [httprobe](https://github.com/tomnomnom/httprobe/releases). So same thing as before.

We're using the following

`cat domains | httprobe | tee hosts`

Wait! There're two commands there! No way! 

Well, the first one is from him, but the second might be there in you bash binaries and you have never notice. This tee command splits the same output in two making one goes to the file and the other one goes to your screen. After that, if you're willing to do that again with the same file, use `tee -A`.

Now we're going to use the `meg` one!

So, with

`meg -d 1000 -v`

The subdomains will be outputed to a folder ./out that will contain a lot of folders with normally two files, one with the http and the other one with the https.

Also there's all the output into the index file!

Now he wanned to grep like:

`grep -Hnri uberinternal * | vim -`

Where grep is to find what's into the passed file, but with the start (*) all of them are being searched. Also, each flag means:

- H is for diplay the file name
- n is "display the line number"
- r is for recursive
- i is for case insensitive

And the pipe to `vim -` tells the bash to open it in a vim file.

### VIM small talk

With vim with just press ESC and slash and the word for searching `/uberinternal` and hit N to go to the next result of the search.

On Vim, after pressed esc and the `:` we can relate commands over the text. The current file in vim is represented as with percent `%`. The BANG `!` means run in a shell.

So, first we can sort the cotent with 

`:%!sort -u`

We can actually do the reverse grep and remove all that we don't want with

`:%!grep -v <The unwanted string>`
`:%!grep -v Content-Sec` - v is invert or remove
`:%!grep -vi csp` - here the i is for case insensitive

## More vim, more tools

Now we're about to see another tool called [gf](https://github.com/tomnomnom/gf) that extends for grep for and do a nice job on grep 'ing nice patters like base64 and others.

So we installed it to do:

`gf base64 | vim -`

on the current directory to filter base 64 apperences.

In vim, we're going to use awk programming language to apply functions to patterns matching.

So esc, and

`:%!awk -F ':' '{print $3}`

Also a could use the search and replace for fim

`:%s/you-search/what-to-replace`

So, to remove the duplicates we do

`:%!sort -u` for unique

We know that if we want to decode we just need to

`base64 -d "THE ENCONDED LINE"` or

`echo "THE ENCODED LINE" | base64 -d`

So we can use the vim for it with xargs that takes every line an executes some command over it.

For this case we're using

`:%!xargs -n1 -I{} sh -c 'echo {} | base64 -d'`

where -n is for one by one, the -I{} explicits a placeholder for this line of data so we can use sh -c to execute any shell command dependent to the content of each line.

------------------------------

Now we can use the html-tool from tomnomnom - Damn hard to make it work. But what it does is sort of recieve some files and it will parse things out - like read the domain and parse it to you "i gues"

We can use it like:

`find . type f | html-tool attribs src | grep '\.js$'` 

to find all js files in the list of files

