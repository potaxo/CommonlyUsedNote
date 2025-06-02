# UsefulGitCommands

## Status of checkout and normal
- When you checkout a branch, it will show the status of the branch.
- But when you checkout a file, it will not show the status of the file.
> This is why when you use `git branch --show-current`, you get blank. You also can't use `git push` or `git pull` in that state.
>
> It will not show the status of the file, but it will show the status of the branch.
- To getout of this state, you can use `git checkout <branch-name>` to switch back to a branch.

## Rollback to a specific commit
- First, in specific branch, use `git log` to find the commit hash you want to rollback to.
- Then, use `git reset --hard <commit-hash>` to rollback to that commit.
- In the end, you can use `git push origin master --force` to update the remote repository with the changes.