
# Create and push local branch to remote

git checkout -b <branch-name>
git push -u origin <branch-name>   


# Add staged files to the last commit
git commit --amend -no-edit

# Discard ALL changes in your local branch
## Option 1
git reset --hard

## Option 2
git restore .

# Discard  changes in a branch
git checkout <branchName>

# List local branches
git branch 

# Stash a single file
git stash push <file>

# Git locate config file
git config --list --show-origin    

# Fixup a commit V2.6
git commit --fixup=<SHA_commit-to-fix>
git rebase --autosquash <SHA_squash-commits-after-this-one>

To avoid husky verifications:
 --no-verify

# Add aliases
 git config --global alias.<alias> <command>
 Ex:  git config --global alias.p pull
 Ex:  git config --global alias.pf 'push --force-with-lease'

# Check all aliases
 git config --list | grep alias

# Cherry pick
git cherry-pick --no-commit <SHA_commit-to-pick>
 
# Create new local branch
git checkout -b ＜new-branch＞

# Rename branch
git branch -m old-branch-name new-branch-name

# Push branch to remote repo
git push -u origin <branch-name>

# Delete local branch
git branch -d branch-name

# Delete local branches without remote branch
git pull --prune
git branch -vv | grep 'gone]' | awk '{print $1}' | xargs git branch -D

# Change the base of a branch
git rebase --onto <new-base> <old-base> <working-branch>

# Get History from a deleted file
git log --all --full-history -- <path-to-file>
