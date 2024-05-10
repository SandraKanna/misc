# Github basics

## Intro
Git is a collaborative tool for source code management (SCM) that enables dev teams to track modifications and manage changes to their source code repositories effectively. By default, a Git repository starts with a main branch, which acts as the primary area for development and represents the official project history. Commits are then added as the work progresses in order to record and manage the modifications.

## Initialisation:
Allows us to start tracking a new or existing project with Git.
* Local Repository:
    1. Initialize the repository:  git init (run from the project directory). This command creates a new subdirectory named .git that contains all of your necessary repository files — a Git repository skeleton. It sets up the Git infrastructure.
    2. Add files to the staging area: git add <filenames> (one by one) or git add . (for all)
    3. Create first commit:  git commit -m "Initial commit”. This sets the initial project state (files start being tracked).
* GitHub (Remote Repository):
    1. Click on "New repository" button. Recommended: add an .gitignore file with the specified untracked files to ignore and then do the initial commit.
    2. Copy the URL of the new repository from GitHub.
    3. Link your local repository to GitHub: git remote add origin <repository-URL>.
    4. Push your commits (sync remote and local): git push -u origin main. The -u flag sets the upstream (tracking) reference for your local branch.

## Commits: 
A commit is a snapshot (like a copy) of the entire working directory at a specific point in time. Each commit records changes from its previous commit (except for the initial commit 0), and this is often referred to as a "delta" or "difference." These deltas make Git very efficient at storing changes between commits.  
Commands example:
1. Add the files to be included in the commit (staging area): git add <filenames>
2. You could also remove files that were added unintentionally: git rm <filenames>
3. Update the repository with the new files/changes: git commit -m “brief description of the changes”

## Status:
- git status (to know which changes have been commited and which haven't)
- git diff (to look at current changes in the working environment, to check changes in commits, to compare branches.)

## Branches:
It’s recommended to always create a branch when adding changes to the main project. A branch isolates the development work **without affecting other branches** in the repository. They’re used for example to code one specific standalone part of the program (a feature). Each branch can have its own series of commits. To move from one branch to another (or create one if it doesn’t exit yet) we use checkout or switch. See branches as pointers to a specific commit, and checkout allows us to move that pointer to the commit we want to work on.  
Commands:
* git checkout branch_name: This command is used to switch your current working directory to the state of the branch named branch_name. If branch_name does not exist, it will result in an error unless you also include -b, which will create a new branch and switch to it.
* git checkout -b branch_name: This creates a new branch based on the commit you are currently on and immediately switches to it.
* git switch branch_name: also for switching branches

## Moving through commits: relative refs
Commands example:
- git checkout branch_name (to go to one of the branches/commits)
- git checkout branch^ (move upwards, to its parent. Also possible: branch^^^…)
- git checkout branch~4 (move 4 times upwards)
- git log (to see the commit log history)

## Reassign a branch to a commit:
Command example:
- git branch -f BugFix main~3 (moves by force the main branch to 3 parents behind)

## Stash:
Commands:  
- git stash: temporarily “hides” the changes we’ve done  to the current branch so we can work in another branch as we cannot move around branches if there are non committed changes in the current branch.
- git stash pop: retrieves the “hidden” work once we go back to that branch

## Merge: 
When we merge, git combines branches by creating a special commit containing the latest commits of the 2 branches being merged.  
Commands example:
1. Create a new branch: git checkout -b BugFix
2. Register the change: git commit -m “creating a branch to fix bugs of one part of main”
3. Go to the main branch: git checkout main 
4. Merge main and the new branch (add the modifs of the branch): git merge BugFix 

## Rebase:
Rebase also combines branches, but it rewrites (copies) the commit history (a set of commits) in order to present a linear sequence of commits. It allows for a cleaner commit log (more readable).  
Commands example:
1. Create a new branch: git checkout -b BugFix
2. Register the change: git commit -m “creating a branch to fix bugs of one part of main”
3. Move work from cur branch to main: git rebase main
4. Finish the rebase so the BugFix branch is now the main: git rebase BugFix (applying changes from one branch onto another)

## Resolving conflicts:
When merging  2 branches that have changed the same part of the same file  a conflict occurs. Git is unable to automatically resolve differences in code between the two commits. To solve this:
- git status (to identify the files with conflicts)
- Open the conflicting files and edit/fix the code (Git already marks the conflicting parts)
- Decide how to integrate the changes from both branches (choose one branch over the other, merge the content of both sides, or make new changes).
- git add (to mark the conflicts as resolved)
- Repeat until all conflicts are resolved then : git commit -m “conflicts resolved”
- Before pushing the changes, ensure that the code runs as expected.

## Reversing changes:
**git reset** (working solo): reverses changes by moving a branch reference backwards in time to an older commit. It effectively erases the subsequent commits from the branch's history (in the local repository).  
Command example: git reset main~1  
Options: 
- --soft: the files are not touched, only the commit history
- --mixed: the files are unstaged (excluded from next commit) but changes are preserved
- --hard: erases everything (no way back)

**git revert** (working in team): reverses changes by creating a new commit, this way the commit history is preserved and others can still work with the same branch.  
Command example: git revert BugFix

## Sharing modification with a team:
Once the local changes have been committed and pushed, pull requests (PR) allow you to tell others in your team about these changes made in a branch of the GitHub repository.  
Here is how to do it:
1. Commit your local changes: git commit -m “commit description”
2. Push your branch: git push origin branch_name.
3. Create Pull Request: Go to the repository page on GitHub and click the "Compare & pull request" button next to your branch.
4. Add title and description for the PR
5. Select team members to review the changes.
6. Create the Pull Request: Once the PR is reviewed and approved, you can merge it into the main branch.
7. Merge and Close: Merge the PR, incorporating the changes into the main branch, and close the pull request.


### To go further on how to use git:
- Pro Git book: https://git-scm.com/book/en/v2
- Learn git branching: https://learngitbranching.js.org/
