# Semo Private Design

There are many ways to do all this, find the most efficient way, make some notes and do a PR if you wish to.

### Getting Up-to-date with MITRE's Insecure eCTF Reference Design Repo

## First get main up-to-date

- Switch to main branch - `git switch main` - because main branch is same and should always be synced with the reference design
- `git pull upstream release` - because the reference design repository link is in the upstream/release
- `git push`


## Then get beardy up-to-date

NOTE: beardy is an example test or dev branch - similarly you may handle your own branch. After this there will be a section that shows how to create a new branch and set it up with you updated code.

- `git switch beardy`
- `git merge upstream/release`
- `git add` - in case there were any updates locally which we want to add and commit to the beardy branch.
- `git commit -m "updating beardy"`
- `git push origin beardy`


## We cloned reference design in a different local folder, and now we will merge those changes to our private semo-design repo:

#### In the dev-changes directory where we made some code-development changes to the reference design: 

- `git fetch origin`
- `git merge origin/release`
- `git checkout -b dev`
- `git add .`
- `git commit -m "Dev Changes Migration"`
- `git remote add semo-design C:\Users\sarta\Desktop\2025-ectf-insecure-example` - this path in the end is the path to the local directory of our semo-design private repository
- `git push semo-design dev`

#### In the semo-design private repo's local directory:

- `git fetch origin`
- `git checkout dev`
- `git merge main`
- `git push origin dev`
