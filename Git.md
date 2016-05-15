### Show branches present on local but not on remote
```git remote prune origin --dry-run```
 
### Prune
```git remote prune origin```

#### ```git pull development``` (or whatever is the base branch)

#### Delete all local branches which have been merged to the current(*, the branch currently you are in) branch
```git branch --merged | grep -v "\*" | xargs -n 1 git branch -D```

*How can we use prepare-commit-msg to solve our problem?*

    As per our development process the name of the feature branch must be the ISSUE_ID of the JIRA Project
    The format of the commit message is ISSUE_ID+:.+
    Since the branch name is same as that of the ISSUE_ID we can prepend the branch name to the commit message
    We can write a prepare-commit-msg hook script as below
 
```	
#!/bin/sh
#
# Automatically adds branch name and branch description to every commit message.
#
 
NAME=$(git branch | grep '*' | sed 's/* //')
DESCRIPTION=$(git config branch."$NAME".description)
 
echo "$NAME"':'$(cat "$1") > "$1"
if [ -n "$DESCRIPTION" ]
then
   echo "" >> "$1"
   echo $DESCRIPTION >> "$1"
fi
```
