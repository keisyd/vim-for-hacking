
# Linux commands

Not everytime we feel like working on reinventing the wheel. Thats when pre-existing tools come handy. Lets take a look at some:

# grep


# Find

Its and interesting tool. 
> | \
 
> find /var/log -type f -not -name ¨*.log.gz¨

> find /var/www/site -type d -exec chmod 0755 {} \;

> find /tmp -type f -size 1024c

> find . -type f -size +1M

> find . -type f -size +1M -size 27M

> find 
### 0 - bandit0

### 1 - boJ9jbbUNNfktd78OOpsqOltutMc3MY1

### 2 - CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
### 3 - UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
### 4 - pIwrPrtPN36QITSp3EQaw936yaFoFgAB
### 5 - koReBOKuIDDepwhWk7jZC0RTdopnAYKh 

### 6 - DXjZPULLxYr17uwoI01bNLQbtFemEgo7

> cat `find / -user bandit7 -group bandit6 -size 33c 2>/dev/null`

> cat $(find / -user bandit7 -group bandit6 -size 33c 2>/dev/null)

### 7 - HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
Simple grep

### 8 - cvX2JJa4CFALtqS87jk27qwqGhBM9plV

The first crazy sollution I ended up with was:
> sort -u data.txt | xargs -n1 -I{} sh -c 'grep -c {} data.txt | grep -v 10 && echo {}'

I'm not proud of it, but is a nice usage of the xargs flags. 
The ideal command is, of course:

> sort data.txt | uniq -u

I tried to do better with a linear approach:

> for w in  `sort data.txt`; do previous=$current; current=$next; next=$w; if [[ $previous != $current ]]; then if [[ $current != $next ]]; then echo $current; fi; fi;  done

Using time before them is interesting to see that I'm not even close to the ideal solution :D.

```bash
$ time sort data.txt | uniq -u
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR

real    0m0.003s
user    0m0.003s
sys     0m0.001s

$ time sort -u data.txt | xargs -n1 -I{} sh -c 'grep -c {} data.txt | grep -v 10 && echo {}'
1
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR

real    0m0.253s
user    0m0.264s
sys     0m0.077s

time for w in  `sort data.txt`; do previous=$current; current=$next; next=$w; if [[ $previous != $current ]]; then if [[ $current != $next ]]; then echo $current; fi; fi;  done
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR

real    0m0.017s
user    0m0.012s
sys     0m0.005s

```

### 9 - UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR

> strings data.txt | grep "==="

### 10 - truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk

> cat data.txt | base64 -d


### 11 - IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
### 12 - asdasdas!"
### 13 - 
### 14 - 
### 15 - 

```bash


```