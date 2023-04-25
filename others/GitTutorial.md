# Git Tutorial

## Configure Git
```
git config --global user.name "your_username"
git config --global user.email "your_email"
git config --global init.defaltBranch main
```
*Use `--global` option to set username and email for all repositories on your system.*

## Initialize Git
```
git init
```  
*Initialize the working directory to be monitored by git*  
```
git status --short
```  
*Use `status` to see the commit and stage state of the current git directory, `--short` option is optional, which summarizes the output.*
#### Short status flags
 - ?? : untracked
 - A  : added to stage
 - M  : modified
 - D  : deleted

## Git staging environment
```
git add file_name
```  
*Adding file `file_name` to the git staging environment*  

You can add all files in the current directory to the git staging environment with the following command.  
```
git add --all
```  
You can substitude `--all` with `-A`.  

## Git commit
```
git commit -m "message"
```  
Commit staging environment to the repository  
*Add a `-a` option to skip staging*  

View commit history
```
git log
```

## Git branch
Create a new branch  
```
git branch branch_name
```  
*Remove the `branch_name` in the command to list all branches*  
*Remove a branch by using `-d` option and followed by the `branch_name`*
Move to a branch
```
git checkout branch_name
```  
*Add a `-b` option to create a new branch if it doesn't exist*  
### Merging branches
To merge the current branch with `branch_name`  
```
git merge branch_name
```  
*When two branches both have changed, the merge command will encounter a conflict which will halt the merging proccess*  

## Github and Git
Adding a remote repository in github
```
git remote add origin https://github.com/user/repo.git
```  
Remove origin
```
git remote remove origin
```  
### Pulling from github
`pull` consists of two seperate commands: `fetch` and `merge`  
```
git fetch origin
git status
git log origin/master
git diff origin/master
```  
```
git merge origin/master
```  
`pull` fetches the **online** repo, compares it to the **local** one, and merges the local repo with the online repo.  
```
git pull origin branch
```  
### Pushing to the `origin`
```
git push --set-upstream origin branch
```  