# Github basics

## Initialisation:
Start tracking a new project with Git in a repository called repo1 that will be hosted in git@github.com:myorganization.
* From local Repository:
    1. Create the local directory for repo1:
        - mkdir repo1
        - cd repo1 
    2. Initialize repo1:  
        - git init
        - git remote add origin git@github.com:myorganization/repo1.git
        - git remote -v (to verify the remote and local are synced)
    3. Add first files to the staging area if needed: 
        - One by one: git add .gitignore readme.md…
        - Add all the files: git add .
    4. Create first commit so files start being tracked.
 
* From GitHub:
    1. Create the remote repository:
        - Click on "New repository" button. 
        - Optional: add a .gitignore file with the specified untracked files to ignore and then do the initial commit.
    2. Copy the URL of the new repository from GitHub:
        - Click on the button “Code” 
        - Copy: git@github.com:myorganization/repo1.git
    3. Link the local repository to GitHub:
        - git remote add origin git@github.com:myorganization/repo1.git
        - git fetch origin (retrieves the remote content)
        - git remote -v (to verify the remote and local are synced)
    4. Create first commit so files start being tracked.
 
## Commits: 
A commit is a snapshot of the entire working directory at a specific point in time. Each commit records changes from its previous commit (“deltas”). These deltas allow to keep track of the modifications done to the very first version of the project repository.

*Best practice for the first commit:
    1. Create a main branch:
        - git checkout -b main
    2. Push your commits to sync remote and local repos: 
        - git push -u origin main.

After the first commit you can just:
- Add your files with: git add . (or git add file.extension) 
- git commit -m “description of the changes”
- Push your commits to sync local and remote: git push

* Best practice: add one commit per change so the tracked record is cleaner

## Track modifications:
To know what files or sub directories have been added or not to the staging area (committed):
- git status

To look at current changes in the working environment or compare branches:
- git diff

## Branches:
A branch is like a copy of the main project on which we can safely modify the code. It isolates the development work without affecting other branches in the repository. Each branch can have its own series of commits.  
To create a new branch:
- git checkout -b branch_name
To delete a branch:
- git branch -d branch_name

Start your modifications, add your files, commit them and push them to be synced to remote repo. Merge/rebase main and delete the branch if not needed any more.

*Best practice:
Create a branch every time a part of the main project need to be modified.  
Example:
1. Teammate1 works on feature_1:
    - git checkout -b feature_1
    - Develop the code for feature_1
    - Add, commit and push the changes
    - Merge/rebase to main when all changes OK and delete the branch if not needed
2. Teammate2 works on feature_2:
    - git checkout -b feature_2
    - Develop the code for feature_2
    - Add, commit and push the changes
    - Merge/rebase to main when all changes OK and delete the branch if not needed
3. Teammate1 debugs feature_1:
    - Go to the main branch: git checkout main 
    - Create the debug branch: git checkout -b debug_feature_1
    - Fix the code of feature_1
    - Add, commit and push the changes
    - Merge/rebase to main when all changes OK and delete the branch if not needed
4. Teammate2 test another version of feature_2:
    - Go to the feature_2 branch: git checkout feature_2
    - Create the v2 branch: git checkout -b feature_2_v2
    - Develop the code of feature_2_v2
    - Add, commit and push the changes
    - Choose which version to use with main then merge/rebase

## Rebase:
Rebase combines branches by rewriting (“copying”) the commit history of one branch onto another. This simplifies project history (cleaner log) by creating a linear progression, making it easier to follow without the divergences caused by merge commits.
- git rebase branch_name

Example: rebase feature_1
1. Go to the main branch and retrieve the latest version:
    - git checkout main 
    - git pull
2. Go to feature_1 and apply its changes to main:
    - git checkout feature_1
    - git rebase main
3. Sync the changes with remote repo:
    - git push origin feature_1

If conflicts arise, identify the conflicting files, fix the errors, then continue the rebase:
- git add <resolved-file>
- git rebase --continue
- git push origin feature_1 --force

## Merge: 
Merging also combines branches, but it creates a special commit containing the latest commits of the 2 branches being merged, which overloads the log history but preserves the history of both branches to come back to them later if needed.  
Example 1: merge the first feature
1. Go to the main branch: git checkout main 
2. Retrieve the latest version of main: git pull
3. Merge main and feature_1: git merge feature_1

Example 2: use the second version of feature_2_v2
1. Go to the feature_2 branch: git checkout feature_2
2. Merge feature_2 and feature_2_v2: git merge feature_2_v2
3. Then merge main and feature_2

## Moving through branches
Switch your current working directory:
- git checkout branch_name
- git switch branch_name

## Moving through commits
Move using relative references: move to “parent” commit
- git checkout branch^ (move upwards, to its parent. Also possible: branch^^^…)
- git checkout branch~4 (move 4 times upwards)
- git log (to see the commit log history)

Example:
1. Go from feature_2 to its parent (main): git checkout feature_2^
2. Go from feature_2_v2 to main: 
    - git checkout feature_2_v2^^
    - git checkout feature_2_v2~2

If you have non committed modifications you cannot move around branches:
    1. Add, commit and push the changes, or
    2. Stash (“hide”) your changes so you can move to another branch

## Reassign a branch to a commit:
Command example:
- git branch -f BugFix main~3 (moves by force the main branch to 3 parents behind)

## Stash:
Commands:  
- git stash: temporarily “hides” the changes we’ve done  to the current branch so we can work in another branch as we cannot move around branches if there are non committed changes in the current branch.
- git stash pop: retrieves the “hidden” work once we go back to that branch

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

## Sharing modification with a team: Pull requests
Once the local changes have been committed and pushed, pull requests (PR) allow to tell others about these changes made in a branch of the GitHub repository. 
- Go to the Github repository and branch you modified (there usually be a notification to open a PR)
- Open a PR so the work can be discussed and reviewed by all collaborators
- add follow-up commits before the changes are merged into the base branch.

How to:
1. Commit your local changes: git commit -m “commit description”
2. Push your branch: git push origin branch_name.
3. Create Pull Request: Go to the repository page on GitHub and click the "Compare & pull request" button next to your branch.
4. Add title and description for the PR
5. Select team members to review the changes.
6. Create the Pull Request: Once the PR is reviewed and approved, you can merge it into the main branch.
7. Fix conflicts if any and merge the PR to incorporate the changes into the main branch
8. Close the pull request.

### To go further on how to use git:
- Pro Git book: https://git-scm.com/book/en/v2
- Learn git branching: https://learngitbranching.js.org/
