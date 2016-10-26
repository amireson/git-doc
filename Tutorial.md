# Command line + git tutorial

Andrew Ireson <br>
2016-10-22

---

## Objectives:

Learn the command line and git commands that are needed to practically use version control. We use version control for two main reasons: 1) is to keep a track of our own work for ourselves, in a way that let's us keep everything organized and also revert to older versions if we wish to. 2) is to share common files with others - in particular scripts/model code that is being co-developed. If you can get over the learning curve, I think you will appreciate git.

## Lesson 1: navigating in the terminal

Open up a terminal (on Windows, open the "git bash" terminal). Type the following, one line at a time:

```
cd Desktop
mkdir Junk
cd Junk
ls
pwd
cd ..
pwd
cd ..
pwd
cd ~/Desktop/Junk
pwd
```

Do you understand what you did? Once you understand each line, try to create a new folder on your desktop call Bob. What the does the command `cd` on it's own do?

## Lesson 2: Copying, moving and deleting

In the terminal, change in the folder Junk on your desktop, using the commands you learnt above. Type the following, one line at a time:

```
touch Hello.txt
ls
pwd
mv Hello.txt Goodbye.txt
ls
cp Goodbye.txt Hello.txt
ls
rm Goodbye.txt
ls
rm Hello.txt
ls
cd ..
pwd
ls
rmdir Junk
ls
pwd
```

Make sure you understand all the commands. Now remove the folder on your desktop that is called Bob, and recreate a folder called Junk, and within Junk create three empty files call "a.txt", "b.txt" and "c.dat". What happens if you now try to delete the folder Junk with `rmdir`?

## Lesson 3: Wildcards

In the terminal, navigate into the folder Junk on your desktop, and type the following, one line at a time:

```
ls
ls *.txt
ls *.dat
mkdir TextFiles
mkdir DataFiles
mv *.txt TextFiles/.
mv *.dat DataFiles/.
ls
ls TextFiles
ls DataFiles
```

Make sure you understand all the commands. Can you move all the .txt files back into the folder Junk, and then delete the folder "TextFiles". 

## Lesson 4: git for you and only you

Now you know enough command line functions to be able to work effectively with git. You will have to install git first (google, download, install). After installation, you must specify your name and email using the commands:

```
git config --global user.name "Andrew Ireson"
git config --global user.email "andrew.ireson@usask.ca"
```

When this is done, navigate to the folder Junk on your desktop. Next, delete everything in this folder, and create the following empty files: "a.txt", "b.txt", "c.dat". Now type the following, one line at a time:

```
ls
ls -a
git init
ls -a
```

What just happened?

```
git status
ls -a
rm -rf .git
ls -a
git status
```

What just happened?

```
git init
git status
git add .
git status
git commit -m "First commit"
git status
git shortlog
```

What just happened?

git is used to track versions of the files in a particular folder. You have to periodically add/commit these files to the git repository. The git repository is in a hidden folder called ".git". If you delete that folder, you delete your repository. The commands that you routinely use for working with git are:

```
git init
git status
git shortlog
git add 'filename'
git commit -m "Commit message"
```

Now let's make some changes and commit these. Do the following:

```
cat a.txt
ls -a > a.txt
cat a.txt
```

What just happened?

```
git status
git add a.txt
git commit -m "Edited a.txt"
git status
git shortlog
```

Did you follow that? If so, edit the file "b.txt" and commit those changes to the repository. Can you see how `git shortlog` lists your commit messages? This can be very helpful as a reminder of the last thing you did when you come back to work on a project.

This is 99% of my workflow with git. I use these commands 3 or 4 times a day on a particular project, to keep everything organized. If I'm editting a word document, I never have to change the name (e.g. Proposal.docx, ProposalR1.docx, ProposalR1_HW.docx, etc etc) because I just keep committing the edits into the git repository. The final command to learn, which I used very rarely, is `checkout`. Do the following:

```
df -h > c.dat
cat c.dat
rm a.txt
git status
```

You should see that "c.dat" needs to be committed. However, I also deleted a.txt by mistake, and I want that back. You can't undo `rm`, but with git you can bring back the last committed version. Let's do that:

```
ls
git status
git checkout a.txt
ls
git status
git add .
git commit -m "Editted c.dat"
```

If you can master these commands, you'll be able to use git routinely in your work and it will make your life way easier!

## Lesson 5: git for us

Do not attempt this until you have mastered Lesson 4. Here we will learn how you can get your hands on my repositories, edit them and even push them back to the server so that I get your edits. This will be very useful forscripts we are jointly working on. We will share tools using this method, such as my python script to download WISKI data and my python script to download Environment Canada data.

I have a shared folder on my linux server that you have access to. The computer name is `water-aireson.usask.ca`, and the folder is `/home/Repo`. You can get onto my server using the command `ssh`. Let's first try that. In the steps below replace abc123 with your nsid. You need your password for my server.

```
ssh abc123@water-aireson.usask.ca
cd /home/Repo
ls
exit
```

Note, when you use `ssh` it is necessary to type `exit` to return to your local machine. These commands listed the shared git repositories on my server. You can also list them with the single line:

```
ssh abc123@water-aireson.usask.ca ls /home/Repo
```

Now we will "clone" the repository VG onto your desktop. First, navigate to your desktop. Then type

```
ls
git clone abc123@water-aireson.usask.ca:/home/Repo/VG
ls
cd VG
git status
```

Now you have a fresh copy of the repository. You can delete it anytime - this will not affect the server. You can add new commits to your local copy, and generally use all the commands in Lesson 4. Now a few different things could happen, and we're into advanced git usage. This will be dealt with with a later lesson. For now, this is enough information for you to work effectively with git.

## Reference: working with a remote repository

When you are working with a remote repository you will need the following commands:

`git clone` Make a new copy of the remote repo in the current folder. See Lesson 5.

`git remote -v` Print information about the remote repository

`git fetch origin` Brings information about the current version of the repository to the local computer. Then you can type

`git diff --name-status origin/master master` Which lists the files that have changes. Prefixes mean: A - added, M - modified, D - deleted. If you want to see the actual differences within each file omit `--name-status`.

`git merge origin/master master` Brings changes on the server version to your local version. Only run this after you have run the `fetch` command above.

`git push origin master` Sends your changes in your local repository to the version on the server. 

In all cases, if there are clashes (e.g. if two people have edited the same file) you will be notified and the action will not be completed.

### Note on branches

It might help with the above to understand a little bit about branches and the names "origin" and "master". When you work with git you are always in a particular named branch. By default this is "master" and I have not found any need to change this - though you can. You can also switch between your local branches with `checkout`, though again I haven't done this. To see your branches you can type 

```
git log --oneline --decorate
```

The remote repository was created empty and was connected to a local repository with the command `git add remote origin ADDRESS`. Here origin defined the name of the remote repository. You can use something other than origin, but don't. Now, when you push your local repo to the remote, with `git push origin master` you create a branch on the remote called `origin/master`. You can always use fetch then diff to figure out differences between the local and remote repository.
