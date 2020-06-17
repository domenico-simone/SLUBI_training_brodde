# Setting up git

- Making an account https://github.com
username: La2Bro
psw: *N....K.......2*


- installing git on mykopat cluster mgrid
 *#following tutorial http://swcarpentry.github.io/git-novice/02-setup/index.html*



```bash
conda search git

conda install git

git --version
#git version 2.27.0

git config --global user.name "La2Bro"
git config --global user.email "laura.brodde@slu.se"
git config --global core.editor "atom --wait"

git config --list
# http.sslverify=true
# http.sslcapath=/nethomes/lade0002/miniconda3/ssl/cacert.pem
# http.sslcainfo=/nethomes/lade0002/miniconda3/ssl/cacert.pem
# user.name=La2Bro
# user.email=laura.brodde@slu.se
# core.editor=atom --wait
```
- Creating a reposition

```bash
cd /nfs4/my-gridfront/mykopat-proj3/mykopat-pineg/
mkdir calling
cd calling/
git init
ls -a
# .  ..  .git
git status
# On branch master
#
# No commits yet
#
# nothing to commit (create/copy files and use "git add" to track)
```

#### 04.06.20 learning session with Domenico

- GIT, a tool for version control

- set up of new, shared repository:
 - import an exsiting repository
 - "readme.md", created in folder
 - --> push an exsiting repository to remote repository on github


- syncronizing local and remote repository --> adding and committing files:
```bash
git add -A  #add files to remote repository
git commit -m  # final committing of files, use -m for adding message of what the commitment includes
```

- check local repository

```bash
git status
# "on branch master", lists untracked files
# then, run to sync files:
git add "file name"
# or for all files:
git add -A / --all

#check status again to list files to be committed

git log  #show made changes

git diff "file name" #diff of  files
#changes accumulate in file until "add" and "commit" is usered

git diff --staged #show changes of file that is alreday staged
```

- commit after every significant change

###### Download
- add & commit are locally
- "push" changes to get the changes files to the public repository
- working alone: using "push"ing as a sort og backup
- worikng in a team: do it after editing, if someone needs to continue working
with that file
- on github: setting up the repository:
 - clone OR download repository as a zip
 -  --> download: can't setup version tracking
 -  --> clone: you'll get the possibility for tracking

```bash
git clone <https:// ... >
```

- working with branches on git
 - before doing changes: run
 ```bash
 git null
 ```
 to make sure you are working on the latest version of the files




###### continue with lections on http://swcarpentry.github.io/git-novice/02-setup/index.html
