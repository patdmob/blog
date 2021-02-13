# My Top X `git` Commands

I'm not going to say that I'm a **git** master. I'm *not*. I still have a lot to learn. But I have been using it every day at work for years. And each year I learn new tricks to make my workflow a bit better. Or I work on a team that does things differently. So over time, I adapt the way I **git** it. 

For the TLDR, here's the condensed list:

1. git branch
2. git commit -am
3. git branch
4. git checkout [branch]
5. git push
6. git pull
7. git add
8. git checkout -b
9. git branch -d
10. git status
11. git stash
12. git stash pop
13. git reset —hard
14. git diff master..[branch]
15. git rebase master
16. git log
17. git clone
18. git remote prune origin
19. git tag
20. git describe —tags

The biggest difference I've seen is between the way that teams interact with `git`. Some teams I've been part of use the [gitflow workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow). But most use the more simple [feature branch workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow). 

I was curious how I now use `git`. So I used google and hacked together the following command:

### CMD to Identify Most Used Git Commands
```bash
cat ~/.zsh_history | \ 
grep -o 'git.*' | \
awk '{CMD[$2]++;count++;}END \ 
{for (a in CMD) print CMD[a]/count*100 "% " a;}' | \
column -c3 -s ' ' -t | \
sort -nr | \
nl
```

What an edited version of output looks like on my home computer:

| Rank | Percent | Sub Command |
|--:|:-:|:--|
|     1 |  12.5%     | push
|     2 |  12.5%     | pull
|     3 |  11.81%  | branch
|     4 |  11.11%  | clone
|     5 |  11.11%  | checkout
|     6 |  9.02%  | commit
| ... | ... | ... |




I don't use my home computer for coding as much (hopefully changing soon). So this doesn't really reflect my workflow when I'm working. 


You can also get more specific by adding the third argument `$3` like this: `awk '{CMD[$2 " " $3]++;count++;}END \ `. 




Feel free to comment below and share your own list or if you think I've left out something really sweet (like cherry pick).