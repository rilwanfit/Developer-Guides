<!----- Conversion time: 4.868 seconds.


Using this Markdown file:

1. Cut and paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β14
* Sun Feb 10 2019 09:44:06 GMT-0800 (PST)
* Source doc: https://docs.google.com/open?id=1JTjzpNjS3d_O40GYynLdbxCJId4pneopgKWxtwYwE6k
* This document has images: check for >>>>>  gd2md-html alert:  inline image link in generated source and store images to your server.
----->


<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 0; ALERTS: 3.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>
<a href="#gdcalert2">alert2</a>
<a href="#gdcalert3">alert3</a>

<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>



## [http://shadowhand.me/better-commits-with-static-review/](http://shadowhand.me/better-commits-with-static-review/)



1.  Installing Git for linux.

[http://tecadmin.net/install-git-on-ubuntu/#](http://tecadmin.net/install-git-on-ubuntu/#)

[http://unix.stackexchange.com/questions/33617/how-can-i-update-to-a-newer-version-of-git-using-apt-get](http://unix.stackexchange.com/questions/33617/how-can-i-update-to-a-newer-version-of-git-using-apt-get)


```
sudo apt-get install git-core

git config --global user.email "you@email.com"
git config --global user.name "username"
```


Where your email should be one that is associated with your account at **github**, **bitbucket**, or wherever you want.



1.  initialize Git in a repository

    ```
git init
```


1.  Add vendor folder into gitignore

echo vendor > .gitignore



1.  Add remote URL to the current Git repo

Git remote add origin git@git.adasd.das.dasd.as.git



1.  How do you revert a commit that has already been pushed and made public?

The git revert command undo the changes by the new commits (with undoing changes). This prevent git from losing history.

_git revert _{_hash_}

# Revert the last commit \
git revert HEAD

git revert HEAD~2..HEAD         revert last two commits

git revert HEAD~3                     revert fourth last commit

git revert -n master~5..master~2        



1.  How do you find a list of files that has changed in a particular commit?

_git diff_-_tree _-_r _{_hash_}

-_r_  List individual files, rather than collapsing them into root directory names only

_git diff_-_tree _-_-no-commit-id --name-only -r _{_hash_}

-_-no-commit-id                  _suppress the commit hashes from output

_--name-only                      _only print file names, instead of their paths



<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Git0.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Git0.png "image_tooltip")




1.  How do you setup a script to run every time a repository receive new commit through push?
1.  How do you configure a git repository to run code sanity check tools right before making commits, and preventing them if the test fails?
1.  What is git rebase? and how can it be used to resolve conflicts in a feature branch before merge?
1.  what are the different ways you can refer to commit
1.  what is git bisect? How can you use it to determine the source of a (regression) bug?
1.  how do you squash last N commits into a single commit?
1.  How to remove .idea folder only in remote that has already been pushed and made public?

git rm --cached mylogfile.log        --------> For File

git rm --cached -r mydirectory      -------> For Directory



1.  how to view the committed files those you have not pushed yet

git log origin..HEAD



1.  **Amending the Last Commit**

    ```
$ git commit --amend -m "New and correct message"
```



Note: Make sure you don't have any working copy changes staged before doing this or they will get committed too. (Unstaged changes will not get committed.)


## Changing the message of a commit that you've already pushed to your remote branch

If you've already pushed your commit up to your remote branch, then you'll need to force push the commit with

git push <remote> <branch> --force \
# Or \
git push <remote> <branch> -f

**Warning: force-pushing will overwrite the remote branch with the state of your local one**. If there are commits on the remote branch that you don't have in your local branch, you _will_ lose those commits.

**Warning: be cautious about amending commits that you have already shared with other people.** Amending commits essentially _rewrites_ them to have different[ SHA](http://en.wikipedia.org/wiki/SHA-1) IDs, which poses a problem if other people have copies of the old commit that you've rewritten. Anyone who has a copy of the old commit will need to re-synchronize their work with your newly re-written commit, which can sometimes be difficult, so make sure you coordinate with others when attempting to rewrite shared commit history, or just avoid rewriting shared commits altogether

[https://www.git-tower.com/learn/git/faq/edit-fix-commit-message](https://www.git-tower.com/learn/git/faq/edit-fix-commit-message)

.


## Documentation



*   [git-commit(1) Manual Page](https://www.kernel.org/pub/software/scm/git/docs/git-commit.html)
1.   `git add -p  `

Interactively add lines of code to the next commit. you will be prompted with each and can approve or disapprove.



1.  The log

A good log format can help you understand individual commits in the context of the repository's history.



1.  gem apt-get install git-smart

Provides the `git smart-pull `command for intelligently integrating remote commits.



1.  gem apt-get install omglog

Show the commit graph with live updating as commits are made, merged and branched.



1.  git reflog
1.  tree .git

Show git directory in flat file format.


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



1.  Rebase

A rebase takes a range of commits and re-commits them on top of a new point. it builds new history from existing commits.



1.  **view the committed files those you have not pushed yet**

**	**git log origin..HEAD



1.  **List all the files in a commit**

**git diff-tree --no-commit-id --name-only -r bd61ad98**



1.  **A nice command to clean up local branches that have been merged into the current HEAD: **

**git branch --merged | grep -v "\*" | xargs -n 1 git branch -d**



1.  **Resolving Git conflicts**

**Rerere**

Rerere stands for "reuse recorded resolution". If this Git feature is turned on, it does two things. First it will record how you deal with conflicts. Secondly, if there is the exact same conflict, Git will resolve it for you. Just like that!


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

**[http://christoph-rumpel.com/2015/07/recover-a-deleted-branch-in-git/](http://christoph-rumpel.com/2015/07/recover-a-deleted-branch-in-git/)**



1.  **Clean up your commits for a pull request**

**[http://christoph-rumpel.com/2015/05/clean-up-your-commits-for-a-pull-request/](http://christoph-rumpel.com/2015/05/clean-up-your-commits-for-a-pull-request/)**



1.  

    


```

```



```

```



```

```



```

```


**http://ryanwinchester.ca/post/set-up-ssh-keys-for-git**

**How to use Github using terminal commands?**

[http://stackoverflow.com/questions/11019839/how-to-use-github-using-terminal-commands](http://stackoverflow.com/questions/11019839/how-to-use-github-using-terminal-commands)


### Colouring the terminal

[http://www.harecoded.com/terminal-tuning-for-git-developers-in-mac-2364711](http://www.harecoded.com/terminal-tuning-for-git-developers-in-mac-2364711)

Control Code Quality

[http://tech.zumba.com/2014/04/14/control-code-quality/](http://tech.zumba.com/2014/04/14/control-code-quality/)

Diff

[http://www.sitepoint.com/understanding-version-control-diffs/](http://www.sitepoint.com/understanding-version-control-diffs/)

Composer without git ignore

[http://www.lornajane.net/posts/2014/using-composer-without-gitignoring](http://www.lornajane.net/posts/2014/using-composer-without-gitignoring)

Broken code

[http://kvz.io/blog/2013/12/29/one-git-commit-hook-to-rule-them-all/](http://kvz.io/blog/2013/12/29/one-git-commit-hook-to-rule-them-all/)


## Hooks

[http://stackoverflow.com/questions/2293498/git-commit-hooks-global-settings](http://stackoverflow.com/questions/2293498/git-commit-hooks-global-settings)

[http://www.sitepoint.com/git-hooks-fun-profit/](http://www.sitepoint.com/git-hooks-fun-profit/)

[https://www.kernel.org/pub/software/scm/git/docs/githooks.html](https://www.kernel.org/pub/software/scm/git/docs/githooks.html)


### Hooks with staticReview

[http://www.sitepoint.com/writing-php-git-hooks-with-static-review/](http://www.sitepoint.com/writing-php-git-hooks-with-static-review/)

[http://www.sitepoint.com/deploy-website-using-laravel-git/](http://www.sitepoint.com/deploy-website-using-laravel-git/)

CREATE BRANCH AND PUSH REMOTELY

[http://stackoverflow.com/questions/1519006/how-do-you-create-a-remote-git-branch](http://stackoverflow.com/questions/1519006/how-do-you-create-a-remote-git-branch)

[http://www.gitguys.com/how-to-recover-a-deleted-a-file-from-my-git-repository/](http://www.gitguys.com/how-to-recover-a-deleted-a-file-from-my-git-repository/)

Viewing Unpushed Git Commits

[http://stackoverflow.com/questions/2016901/viewing-unpushed-git-commits](http://stackoverflow.com/questions/2016901/viewing-unpushed-git-commits)

[http://www.commandlinefu.com/commands/view/2345/show-git-branches-by-date-useful-for-showing-active-branches](http://www.commandlinefu.com/commands/view/2345/show-git-branches-by-date-useful-for-showing-active-branches)

[http://geshan.com.np/blog/2014/12/do-you-git-your-code-follow-this-simplified-gitflow-model/](http://geshan.com.np/blog/2014/12/do-you-git-your-code-follow-this-simplified-gitflow-model/)

[http://stackoverflow.com/questions/6127328/how-can-i-delete-all-git-branches-which-have-been-merged](http://stackoverflow.com/questions/6127328/how-can-i-delete-all-git-branches-which-have-been-merged)


#### Git Delete Last Commit

you have committed junk but not pushed,

git reset --soft HEAD~1 \


HEAD~1 is a shorthand for the commit before head. Alternatively you can refer to the SHA-1 of the hash you want to reset to. _--soft_ option will delete the commit but it will leave all your changed files "Changes to be committed", as git status would put it.

If you want to get rid of any changes to tracked files in the working tree since the commit before head use _--hard_ instead.

Now if you already pushed and someone pulled which is usually my case, you can't use git reset. You can however do a git revert,

git revert HEAD \


This will create a new commit that reverses everything introduced by the accidental commit.


#### Delete all merged branches 

[http://stackoverflow.com/questions/6127328/how-can-i-delete-all-git-branches-which-have-been-merged](http://stackoverflow.com/questions/6127328/how-can-i-delete-all-git-branches-which-have-been-merged)


#### Delete branches name starting with.

git branch --list 'SHIP-3450/*' | xargs git branch -D

 


## Git logs

Browsing the log messages:

**_git log --oneline -5 --author m.rilwan --before "Fri Mar 26 2009"_**


# Commit message

A team's approach to its commit log should be no different. In order to create a useful revision history, teams should first agree on a commit message convention that defines at least the following three things:

**Style**. Markup syntax, wrap margins, grammar, capitalization, punctuation. Spell these things out, remove the guesswork, and make it all as simple as possible. The end result will be a remarkably consistent log that's not only a pleasure to read but that actually does get read on a regular basis.

**Content**. What kind of information should the body of the commit message (if any) contain? What should it not contain?

**Metadata**. How should issue tracking IDs, pull request numbers, etc. be referenced?


## The seven rules of a great Git commit message



1.  Separate subject from body with a blank line
1.  Limit the subject line to 50 characters
1.  Capitalize the subject line
1.  Do not end the subject line with a period
1.  Use the imperative mood in the subject line
1.  Wrap the body at 72 characters
1.  Use the body to explain what and why vs. how

Example:


```
Summarize changes in around 50 characters or less

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. In some contexts, the first line is treated as the
subject of the commit and the rest of the text as the body. The
blank line separating the summary from the body is critical (unless
you omit the body entirely); various tools like `log`, `shortlog`
and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you
are making this change as opposed to how (the code explains that).
Are there side effects or other unintuitive consequences of this
change? Here's the place to explain them.

Further paragraphs come after blank lines.

 - Bullet points are okay, too

 - Typically a hyphen or asterisk is used for the bullet, preceded
   by a single space, with blank lines in between, but conventions
   vary here

If you use an issue tracker, put references to them at the bottom,
like this:

Resolves: #123
See also: #456, #789
```



### Separate subject from body with a blank line

From the git commit [manpage](https://www.kernel.org/pub/software/scm/git/docs/git-commit.html#_discussion):

Though not required, it's a good idea to begin the commit message with a single short (less than 50 character) line summarizing the change, followed by a blank line and then a more thorough description. The text up to the first blank line in a commit message is treated as the commit title, and that title is used throughout Git. For example, [git-format-patch(1)](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-format-patch.html) turns a commit into email, and it uses the title on the Subject line and the rest of the commit in the body.

Firstly, not every commit requires both a subject and a body. Sometimes a single line is fine, especially when the change is so simple that no further context is necessary.

Example: Fix typo in introduction to user guide

Nothing more need be said; if the reader wonders what the typo was, she can simply take a look at the change itself, i.e. use **git show or git diff or git log -p.**

For example:

 \


However, when a commit merits a bit of explanation and context, you need to write a body. For example:

Derezz the master control program \
 \
MCP turned out to be evil and had become intent on world domination. \
This commit throws Tron's disc into MCP (causing its deresolution) \
and turns it back into a chess game. \


Commit messages with bodies are not so easy to write with the -m option. You're better off writing the message in a proper text editor. If you do not already have an editor set up for use with Git at the command line, read [this section of Pro Git](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration).

In any case, the separation of subject from body pays off when browsing the log. Here's the full log entry:

$ git log \
commit 42e769bdf4894310333942ffc5a15151222a87be \
Author: Kevin Flynn <kevin@flynnsarcade.com> \
Date:   Fri Jan 01 00:00:00 1982 -0200 \
 \
 Derezz the master control program \
 \
 MCP turned out to be evil and had become intent on world domination. \
 This commit throws Tron's disc into MCP (causing its deresolution) \
 and turns it back into a chess game. \


And now git log --oneline, which prints out just the subject line:

$ git log --oneline \
42e769 Derezz the master control program \


Or, git shortlog, which groups commits by user, again showing just the subject line for concision:

$ git shortlog \
Kevin Flynn (1): \
      Derezz the master control program \
 \
Alan Bradley (1): \
      Introduce security program "Tron" \
 \
Ed Dillinger (3): \
      Rename chess program to "MCP" \
      Modify chess program \
      Upgrade chess program \
 \
Walter Gibbs (1): \
      Introduce protoype chess program \


There are a number of other contexts in Git where the distinction between subject line and body kicks in—but none of them work properly without the blank line in between.


### **2. Limit the subject line to 50 characters**

50 characters is not a hard limit, just a rule of thumb. Keeping subject lines at this length ensures that they are readable, and forces the author to think for a moment about the most concise way to explain what's going on.

_Tip: If you're having a hard time summarizing, you might be committing too many changes at once. Strive for [atomic commits](https://www.freshconsulting.com/atomic-commits/)(a topic for a separate post)._

GitHub's UI is fully aware of these conventions. It will warn you if you go past the 50 character limit:



<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Git1.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Git1.png "image_tooltip")


And will truncate any subject line longer than 72 characters with an ellipsis:



<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Git2.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Git2.png "image_tooltip")


So shoot for 50 characters, but consider 72 the hard limit.


### **3. Capitalize the subject line**

This is as simple as it sounds. Begin all subject lines with a capital letter.

For example:



*   Accelerate to 88 miles per hour

Instead of:



*   accelerate to 88 miles per hour


### **4. Do not end the subject line with a period**

Trailing punctuation is unnecessary in subject lines. Besides, space is precious when you're trying to keep them to [50 chars or less](https://chris.beams.io/posts/git-commit/#limit-50).

Example:



*   Open the pod bay doors

Instead of:



*   Open the pod bay doors.


### **5. Use the imperative mood in the subject line**

_Imperative mood_ just means "spoken or written as if giving a command or instruction". A few examples:



*   Clean your room
*   Close the door
*   Take out the trash

Each of the seven rules you're reading about right now are written in the imperative ("Wrap the body at 72 characters", etc.).

The imperative can sound a little rude; that's why we don't often use it. But it's perfect for Git commit subject lines. One reason for this is that **Git itself uses the imperative whenever it creates a commit on your behalf**.

For example, the default message created when using git merge reads:

Merge branch 'myfeature' \


And when using git revert:

Revert "Add the thing with the stuff" \
 \
This reverts commit cc87791524aedd593cff5a74532befe7ab69ce9d. \


Or when clicking the "Merge" button on a GitHub pull request:

Merge pull request #123 from someuser/somebranch \


So when you write your commit messages in the imperative, you're following Git's own built-in conventions. For example:



*   Refactor subsystem X for readability
*   Update getting started documentation
*   Remove deprecated methods
*   Release version 1.0.0

Writing this way can be a little awkward at first. We're more used to speaking in the _indicative mood_, which is all about reporting facts. That's why commit messages often end up reading like this:



*   Fixed bug with Y
*   Changing behavior of X

And sometimes commit messages get written as a description of their contents:



*   More fixes for broken stuff
*   Sweet new API methods

To remove any confusion, here's a simple rule to get it right every time.

**A properly formed Git commit subject line should always be able to complete the following sentence**:



*   If applied, this commit will _<span style="text-decoration:underline;">your subject line here</span>_

For example:



*   If applied, this commit will _refactor subsystem X for readability_
*   If applied, this commit will _update getting started documentation_
*   If applied, this commit will _remove deprecated methods_
*   If applied, this commit will _release version 1.0.0_
*   If applied, this commit will _merge pull request #123 from user/branch_

Notice how this doesn't work for the other non-imperative forms:



*   If applied, this commit will _fixed bug with Y_
*   If applied, this commit will _changing behavior of X_
*   If applied, this commit will _more fixes for broken stuff_
*   If applied, this commit will _sweet new API methods_

_Remember: Use of the imperative is important only in the subject line. You can relax this restriction when you're writing the body._


### **6. Wrap the body at 72 characters**

Git never wraps text automatically. When you write the body of a commit message, you must mind its right margin, and wrap text manually.

The recommendation is to do this at 72 characters, so that Git has plenty of room to indent text while still keeping everything under 80 characters overall.

A good text editor can help here. It's easy to configure Vim, for example, to wrap text at 72 characters when you're writing a Git commit. Traditionally, however, IDEs have been terrible at providing smart support for text wrapping in commit messages (although in recent versions, IntelliJ IDEA has [finally](https://youtrack.jetbrains.com/issue/IDEA-53615) [gotten](https://youtrack.jetbrains.com/issue/IDEA-53615#comment=27-448299) [better](https://youtrack.jetbrains.com/issue/IDEA-53615#comment=27-446912)about this).


### **7. Use the body to explain what and why vs. how**

This [commit from Bitcoin Core](https://github.com/bitcoin/bitcoin/commit/eb0b56b19017ab5c16c745e6da39c53126924ed6) is a great example of explaining what changed and why:

commit eb0b56b19017ab5c16c745e6da39c53126924ed6 \
Author: Pieter Wuille <pieter.wuille@gmail.com> \
Date:   Fri Aug 1 22:57:55 2014 +0200 \
 \
   Simplify serialize.h's exception handling \
 \
   Remove the 'state' and 'exceptmask' from serialize.h's stream \
   implementations, as well as related methods. \
 \
   As exceptmask always included 'failbit', and setstate was always \
   called with bits = failbit, all it did was immediately raise an \
   exception. Get rid of those variables, and replace the setstate \
   with direct exception throwing (which also removes some dead \
   code). \
 \
   As a result, good() is never reached after a failure (there are \
   only 2 calls, one of which is in tests), and can just be replaced \
   by !eof(). \
 \
   fail(), clear(n) and exceptions() are just never called. Delete \
   them. \


Take a look at the [full diff](https://github.com/bitcoin/bitcoin/commit/eb0b56b19017ab5c16c745e6da39c53126924ed6) and just think how much time the author is saving fellow and future committers by taking the time to provide this context here and now. If he didn't, it would probably be lost forever.

In most cases, you can leave out details about how a change has been made. Code is generally self-explanatory in this regard (and if the code is so complex that it needs to be explained in prose, that's what source comments are for). Just focus on making clear the reasons why you made the change in the first place—the way things worked before the change (and what was wrong with that), the way they work now, and why you decided to solve it the way you did.

The future maintainer that thanks you may be yourself!


## **Tips**


### **Learn to love the command line. Leave the IDE behind.**

For as many reasons as there are Git subcommands, it's wise to embrace the command line. Git is insanely powerful; IDEs are too, but each in different ways. I use an IDE every day (IntelliJ IDEA) and have used others extensively (Eclipse), but I have never seen IDE integration for Git that could begin to match the ease and power of the command line (once you know it).

Certain Git-related IDE functions are invaluable, like calling git rm when you delete a file, and doing the right stuff with git when you rename one. Where everything falls apart is when you start trying to commit, merge, rebase, or do sophisticated history analysis through the IDE.

When it comes to wielding the full power of Git, it's command-line all the way.

Remember that whether you use Bash or Zsh or Powershell, there are [tabcompletion](https://git-scm.com/book/en/v2/Appendix-A%3A-Git-in-Other-Environments-Git-in-Bash) [scripts](https://git-scm.com/book/en/v2/Appendix-A%3A-Git-in-Other-Environments-Git-in-Powershell) that take much of the pain out of remembering the subcommands and switches.


<!-- Docs to Markdown version 1.0β14 -->
