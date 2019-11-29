---
id: index
title: DDD
---
### Installing Git for linux.

https://git-scm.com/book/en/v2/Getting-Started-Installing-Git

https://launchpad.net/~git-core/+archive/ubuntu/ppa

Configure you environment
```bash
git config --global user.email "you@email.com"
git config --global user.name "username"
```
[this section of Pro Git](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration).

Where your email should be one that is associated with your account at **github**, **bitbucket**, or wherever you want.

### Initialize Git in a repository

```bash
git init
```

### Add vendor folder into gitignore
```bash
echo vendor > .gitignore
```

### Add remote URL to the current Git repo

```bash
git remote add origin git@git.adasd.das.dasd.as.git
```

## Commit

### Interactively add lines of code to the next commit. 
```bash
git add -p
```
you will be prompted with each and can approve or disapprove.

### Amending the Last Commit

```bash
git commit --amend -m "New and correct message"
```

> Note: Make sure you don't have any working copy changes staged before doing this or they will get committed too.
 (Unstaged changes will not get committed.)


### Changing the message of a commit that you've already pushed to your remote branch

If you've already pushed your commit up to your remote branch, then you'll need to force push the commit with

```bash
git push <remote> <branch> --force
```
Or 
```bash
git push <remote> <branch> -f
```

> Warning: force-pushing will overwrite the remote branch with the state of your local one. If there are commits on the 
remote branch that you don't have in your local branch, you _will_ lose those commits.

> Warning: be cautious about amending commits that you have already shared with other people.
 Amending commits essentially _rewrites_ them to have different[ SHA](http://en.wikipedia.org/wiki/SHA-1) IDs, 
 which poses a problem if other people have copies of the old commit that you've rewritten. 
 Anyone who has a copy of the old commit will need to re-synchronize their work with your newly re-written commit, 
 which can sometimes be difficult, so make sure you coordinate with others when attempting to rewrite shared commit
  history, or just avoid rewriting shared commits altogether

[https://www.git-tower.com/learn/git/faq/edit-fix-commit-message](https://www.git-tower.com/learn/git/faq/edit-fix-commit-message)


## Revert

###  How do you revert a commit that has already been pushed and made public?

The git revert command undo the changes by the new commits (with undoing changes). This prevent git from losing history.
```bash
git revert #{HASH}
```

### Revert the last commit
```bash
git revert HEAD
```
### Revert the last two commit
```bash
git revert HEAD~2..HEAD
```

### Revert fourth last commit
```bash
git revert HEAD~3
```

## Rebase

A rebase takes a range of commits and re-commits them on top of a new point. it builds new history from existing commits.

### How to rebase local branch with remote master
```bash
git fetch && git rebase origin/master
```

## Deletion

### Delete Last Commit 

#### If your changes are committed but not pushed?
```bash
git reset --soft HEAD~1
```
- `HEAD~1` is a shorthand for the commit before head. 
    - Alternatively you can refer to the SHA-1 of the hash you want to reset.
    
- `--soft` option will delete the commit but it will leave all your changed files
    - if you want to get rid of any changes to tracked files in the working tree since the commit before head use `--hard`
     instead.
     
> `reset` will remove the commit entirely

#### If your changes are already pushed?
```bash
git revert HEAD
```
This will **create a new commit** that reverses everything introduced by the last commit.


### Delete all merged branches 

[http://stackoverflow.com/questions/6127328/how-can-i-delete-all-git-branches-which-have-been-merged](http://stackoverflow.com/questions/6127328/how-can-i-delete-all-git-branches-which-have-been-merged)

### Delete branches name starting with 'SOME'
```bash
git branch --list 'SOME/*' | xargs git branch -D
```

## Git logs

Browsing the log messages:

```bash
git log --oneline -5 --author m.rilwan --before "Fri Mar 26 2009"
```

```bash
git log --oneline
```

```bash
git reflog
```

### groups commits by user
```bash
git shortlog
```

### how to view the committed files those you have not pushed yet
```bash
git log origin..HEAD
```


## External tools
###  gem apt-get install git-smart

Provides the `git smart-pull `command for intelligently integrating remote commits.

###  gem apt-get install omglog

Show the commit graph with live updating as commits are made, merged and branched.


### cat .git/HEAD


### cat .git/refs/heads/master

If we make a commit to the master branch, the value of `.git/refs/heads/master `will change.

If we checkout another branch, the value of **.git/HEAD **will change.

Every branch, tag and other decoration is just a reference to the ID of a specific commit (calculated with SHA, the secure hash algorithm)

Install tree on mac `brew install tree  `


1.  git reset --hard [SHA]

point the current branch at a specific commit.



1.  Branch

Branches does not store anything related to the history. It is easy to think of a branch as being some separate context in which a range of commits lives.

But a branch is actually only a label. A branch is just like a tag in that it just points to a single commit. The history is implied because it is stored recursively within those commits. 



1.  git log ..[branch-name]

Show log messages in [branch-name] that are not in HEAD (the current commit)



1.  grep -r'<<<<' *

Search for merge conflict markers.



1.  Merge

A merge create a new commit; it does not change previous history.


1.  **view the committed files those you have not pushed yet**

**	**git log origin..HEAD



1.  **List all the files in a commit**

**git diff-tree --no-commit-id --name-only -r bd61ad98**



1.  **A nice command to clean up local branches that have been merged into the current HEAD: **

**git branch --merged | grep -v "\*" | xargs -n 1 git branch -d**



1.  **Resolving Git conflicts**

**Rerere**

Rerere stands for "reuse recorded resolution". If this Git feature is turned on, it does two things. 
First it will record how you deal with conflicts. Secondly, if there is the exact same conflict, Git will resolve it for you.
 Just like that!


### Rerere how

This feature can be activated by setting a git config setting:


```
git config --global rerere.enabled true
```


That's it. From now on Git will remember how you resolve conflicts. You can see that it works when you take a closer look at the Git output after a conflict has been resolved.


```
...
Recorded resolution for 'index.html'.
[master b847e0a] fix conflict
```


And when Git can use an already recorded solution, it will let you know like this:


```
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Resolved 'index.html' using previous resolution.
Automatic merge failed; fix conflicts and then commit the result.
```




## Rerere where

So now that we know what this feature is about, let's think about where this could be useful. If you are working with a feature branch workflow in Git, you will have to merge branches a lot. As a result you ran into same conflicts again and again. This can get really annoying and this is why the rerere feature comes in very handy here.



1.  **Recover a deleted branch**

[http://christoph-rumpel.com/2015/07/recover-a-deleted-branch-in-git/](http://christoph-rumpel.com/2015/07/recover-a-deleted-branch-in-git/){:target="_blank"}



1.  **Clean up your commits for a pull request**

[http://christoph-rumpel.com/2015/05/clean-up-your-commits-for-a-pull-request/](http://christoph-rumpel.com/2015/05/clean-up-your-commits-for-a-pull-request/){:target="_blank"}



**http://ryanwinchester.ca/post/set-up-ssh-keys-for-git**


## Exclude files or folder from merge
https://github.com/RWTH-EBC/AixLib/wiki/How-to:-Exclude-files-or-folder-from-merge



**How to use Github using terminal commands?**

[http://stackoverflow.com/questions/11019839/how-to-use-github-using-terminal-commands](http://stackoverflow.com/questions/11019839/how-to-use-github-using-terminal-commands){:target="_blank"}


### Colouring the terminal

[http://www.harecoded.com/terminal-tuning-for-git-developers-in-mac-2364711](http://www.harecoded.com/terminal-tuning-for-git-developers-in-mac-2364711){:target="_blank"}

Control Code Quality

[http://tech.zumba.com/2014/04/14/control-code-quality/](http://tech.zumba.com/2014/04/14/control-code-quality/){:target="_blank"}

Diff

[http://www.sitepoint.com/understanding-version-control-diffs/](http://www.sitepoint.com/understanding-version-control-diffs/){:target="_blank"}

Composer without git ignore

[http://www.lornajane.net/posts/2014/using-composer-without-gitignoring](http://www.lornajane.net/posts/2014/using-composer-without-gitignoring){:target="_blank"}

Broken code

[http://kvz.io/blog/2013/12/29/one-git-commit-hook-to-rule-them-all/](http://kvz.io/blog/2013/12/29/one-git-commit-hook-to-rule-them-all/){:target="_blank"}


## Hooks

[http://stackoverflow.com/questions/2293498/git-commit-hooks-global-settings](http://stackoverflow.com/questions/2293498/git-commit-hooks-global-settings){:target="_blank"}

[http://www.sitepoint.com/git-hooks-fun-profit/](http://www.sitepoint.com/git-hooks-fun-profit/){:target="_blank"}

[https://www.kernel.org/pub/software/scm/git/docs/githooks.html](https://www.kernel.org/pub/software/scm/git/docs/githooks.html){:target="_blank"}


### Hooks with staticReview

[https://github.com/phpro/grumphp](https://github.com/phpro/grumphp){:target="_blank"}


CREATE BRANCH AND PUSH REMOTELY

[http://stackoverflow.com/questions/1519006/how-do-you-create-a-remote-git-branch](http://stackoverflow.com/questions/1519006/how-do-you-create-a-remote-git-branch){:target="_blank"}

[http://www.gitguys.com/how-to-recover-a-deleted-a-file-from-my-git-repository/](http://www.gitguys.com/how-to-recover-a-deleted-a-file-from-my-git-repository/){:target="_blank"}

Viewing Unpushed Git Commits

[http://stackoverflow.com/questions/2016901/viewing-unpushed-git-commits](http://stackoverflow.com/questions/2016901/viewing-unpushed-git-commits){:target="_blank"}

[http://www.commandlinefu.com/commands/view/2345/show-git-branches-by-date-useful-for-showing-active-branches](http://www.commandlinefu.com/commands/view/2345/show-git-branches-by-date-useful-for-showing-active-branches){:target="_blank"}

[http://geshan.com.np/blog/2014/12/do-you-git-your-code-follow-this-simplified-gitflow-model/](http://geshan.com.np/blog/2014/12/do-you-git-your-code-follow-this-simplified-gitflow-model/){:target="_blank"}

[http://stackoverflow.com/questions/6127328/how-can-i-delete-all-git-branches-which-have-been-merged](http://stackoverflow.com/questions/6127328/how-can-i-delete-all-git-branches-which-have-been-merged){:target="_blank"}

