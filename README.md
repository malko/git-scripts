GIT SCRIPTS
===========
This is some of shell scripts i've wrote to assist me in my daily job with git.
Add them to your path and you will normally be able to call them directly as common git commands such as ```$ git switchbranch``` for example:

git-switchbranch
----------------
Simply get you back to the previous checked out branch (previous branch is retrieved from reflog)

git-modified
------------
check which file where modified in the repository against given branch (default to origin/master)
this is more precise than a diff --stat as it use commits objects to know what were modified.
Sample usage:
```
$ git modified
$ git modified refbranch
$ git modified origin/refbranch
```

git-review
----------
Work exactly like git-modified but instead of returning a list of modified file it display all patches applied one by one.
It is particularly useful when reviewing a feature branch before merging it in the production branch.
Sample usage:
```
$ git review
$ git review refbranch
$ git review origin/refbranch
```


git-mpd
-------
mpd for Merge Push Delete.
sample usage:
```
$ git mpd branchToMerge
$ git mpd origin/branchToMerge
```
will merge --no-ff the branchToMerge into current branch.
If merge is ok then it will push to origin (without any parameter, you should check your push.default git config it will be best if one of currrent, simple or upstream).
If successfully pushed then will check if branchToMerge is local and then remove the local branch.
Finally it will delete the branch from origin

git-weektag
-----------
This one correspond to a particular tag policy.
We use to tag our branch with the last two digits of the year + the number of the week in the year and finally add a third part for the release number of the week (only when there's already a first release in the week).
This script will allow you to retrieve tag already created this week (with the similar policy) and propose you the new tag to set.
available parameters:
``` 
-h help
-v list previous week tags
-f fetch tags before performing the command
-s set the proposed tag
-p with s will push newly created tag
-S set the proposed tag and push it (same as -sp)
```
current usage first check what the script will propose and then create and publish the tag 
```
$ git weektag -fv                                                                                                                                                  
Fetching tags
no tag this week, last tag: no tag found
proposed tag: 13.25

$ git weektag -S                                                                                                                                                  
proposed tag: 13.25
Total 0 (delta 0), reused 0 (delta 0)
To git@github.com:malko/git-scripts.git
 * [new tag]         13.25 -> 13.25

```
be aware that this don't use annotated or signed flag

git-branchstatus
----------------
This command is particularly useful to get a quick overview of a repository. It return the list of all branches (remote branches by default) prefixed by a status marker to let you know which branch are fully merged in your current branch. Here's a sample default output:
```                                                                                                                                            
✔ origin/search
✗ upstream/gh-pages
✔ upstream/tweetcount
```
you can add list of missing commits by branch by adding the -v option:
```
✔ origin/search
✗ upstream/gh-pages
  + b0414be remove node_modules
  + 8a063cf added .gitignore from master
  + f7379f0 remove .jshintrc
✔ upstream/tweetcount
```

supported options:
```
-h Display this help
-v display missing commits from branch that are not fully merged
-l display status for local branches instead of remote branches
-o display only fully merged branches
-n display only not fully merged branches
-p porcelain mode no fancy output "use = prefix for fully merged branch
   = prefix for fully merged branch
   + prefix for branch with missing commits
```

sample usages:
```
// check status of a current branch against a single branch
$ git branchstatus -v featureBranch

// check status of a refBranch against a featureBranch
$ git branchstatus -vp origin/refBranch featureBranch

// check status of current branch against all local branches
$ git branchstatus -vl
```

All those scripts are just public domain unless specified, use them as you want, even if a note to the author is always appreciated.
