**Curriculum**

https://github.com/generalassembly-studio/WDI_Curriculum/blob/91c2f1ad5289936f3a99441c018b0f5a88687fc2/Week_01/original/1_ProjectManangement_UNIX_Git/5_Git/README.md


## Intro to Git


**git**

The purpose of Git is to manage a project, or a set of files, as they change over time.

**github**

It's a social network build around git. I has has completely changed the way we, as programmers, work. Started as a developer's collaborative platform, GitHub is now the largest online storage space of collaborative works.


**Goals**

- Understanding git and github
- Understanding the basics of manging files
- collaborate with others

**Intro / Motivation**

Example of writer. Being a writer is a fairly solitary occupation. Sit at your desk writing your fiction, let's say in MS Word. You are smart enough that you make back up copies of your work, perhaps to an external drive or flash memory stick. At some point your are done, or perhaps done with a chapter, that's when other people come into the picture. For example your editor, You want your editor to review a finished chapter. What do you do? You save a copy, perhaps you name it 'KlingonWars_Chapter1_V_1-12-19-2013.doc', perhaps zip it up, attach it to an email and send it to you editor for review. Editor opens it up, turns on 'track changes' and starts editing your chapter, annotating it with comments. When done, editor saves a copy KlingonWars_Chapter1_V_1-12-15-2013_reviewed_12-2-2013.doc.

That's a pretty straight forward workflow, ther's one document and only two people involved. I'm sure many of you have experienced this in some form, perhaps writing a paper in school with a classmate? Although the workflow is simple and intuitive, what are some of the problems:

- What if you forget making backup copies?
- How do you revert to a previous version
- Having multiple versions of the same document in different files, doc magament becomes a night mare.
- What if a third person enters the picture, this flow breaks down
- Some open questions: How do you consolidate version, edits to a master doc? Who is in charge of it, who owns the doc,
- Can anybody do anything to the doc

As you see, this breaks down with three people, and is far from ideal when only two are involved.

Now enter software programming. Programming is not a solitary activity, it is a highly team based activity, large teams. That's why I love programming: A project is always more fun when you’ve got friends working with you.

Google: 
- "how many people work on Mac OS" - 3rd result, 600
- "how many people worked on angry birds"" - 2nd result - 40

So how do you collaborate in a software project, what kind of workflows? What are some useful practices that have evolved over the years. 

There cvs, subversion, VSS … but consesus is git and github:


** Let's start with git **

Check that git is installed
	
	git --version
	
Create a document. Let's keep it simple. How about a contact file that contains your name and anddress. **A Note about the directory, it becomes your workspace (a.k.a branch), by default, git refers to it as 'master' **
	
	cd Code
	mkdir git_intro 
	cd git_intro
	subl contact.txt -> Add you name and address, save file
	
	
create repo:

	git init
	
add file to repo:

	git add contact.txt
	
commit to local repo:

	git commit -m'my first commit … ever'
	
check log - the history of that file

	git log contact.txt
	
screw up file (or delete) and recover it:

	git checkout -f
	
add email and phone - check status of repo
	
	git status
	
check differences - there will be more usefull ways to examine differences, UI tools

	git diff	
	
commit first change

	git commit -m'added address line'
	

**My personal workflow usually looks like**

	Do some programming.
	git status to see what files I changed.
	git diff [file] to see exactly what I modified.
	git commit -a -m [message] to commit.
	
	
###Now, everything is still local, on your computer. What if your hard drive crashes, or your laptop is stolen? Or you want to share files?

Enter github - let's take a look
show mine, show facebook, perhaps rails or ruby


** You've done this already - anybody knows there github account, password**

go to github.com and create an account

configure user and email:

	git config --global user.name "John Doe"
	git config --global user.email johndoe@example.com
	
or check configuration:

	git config --global user.name
	git config --global user.email
	
Ok, good to go.

#### Cretate repo on github

- name it 'contact_db'

In command line:

	git remote add origin git@github.com:aikalima/contact_db.git
	
Push changes to remote - you may have to loging with user/password

	git push
	
Check if changes made it to github. Now make a local change

	make change
	git commit -m'my comment' .
	git push origin master
	
Did change make it to remote? Now delete your local directory. Panic, it's all gone.

	git clone … (grab url from git repo page)
	
Tada …

#### Collaboration

Assumption: by now, you all have a contact_db repo on git hub.

The real power of Git comes out when you are collaborating with others on a project.

If you own a repo, you can ask others to collaborate with you as contributors. 

Lab - let's collaborate. Get together in pairs of 2, designate persoon A and B. Ask B to approve your contact file, for example by adding a line 'approved by Bob'. B does this on their laptop, not A's. 

What are the steps?

	- Make B collaborator - Under the repo, go to Settings > Collaborators and add using their github username or email.
	
	- B clones contact_db of A on local machine (in a new folder) - git clone
	- B makes changes and pushes to git hub - git commit/push
	- A picks up changes - git pull origin master

Can you figure it out?

### We're done for today.

		

		 		
	
				

	
	
	


Then github:


- create repo
- add files
- change files locally
- commit changes (local)
- push changes (remote)
- check for status and history of files (status/log)
- diff two revision, see what changes
- discard changes




####Objectives

- Understanding source code control systems
- Understand why it's useful, how would it be without one. 

- Get git up and running in your env.
- Have a git account, basic git profile.

**Learn basic concepts:**

- 	Repository
- 	Workspaces (local / remote)
- 	Revisions

**Basic operations:**

- create repo
- add files
- change files locally
- commit changes (local)
- push changes (remote)
- check for status and history of files (status/log)
- diff two revision, see what changes
- discard changes

**Basic collaboration:**

- clone an existing repo
- contribute to a repo

**Discover gist as a great tool to take notes**


####Big picture

A project is always more fun when you’ve got friends working with you, but how do you do it on a coding project?

Enter:  git hub - a service, you can share your coding projects and collaborate with ease!

DIAGRAM - git universe, main components and operations
Ask class: What do do you think are some of the operations?

I'm here, working locally, you want to share code with Bob, how do you do this? How can Bob collaborate on your project?

I do:
Quick tour of existing git repo
Show famous git repos - ruby!

Now let's create our own repos

Why use git if you were not a programmer, what if they were publishing just something. Computer crashing, you never loose work.

####Lab

I do:
keep track of commands on white board

We do:
- Create github account
- Install git
- create repo
- create a file with your address/contact info
- commit file with comment
- push file, see file appear on github
- change file locally, commit,push again, see changes on github
- see history of file
- revert to a previous version

You do:
Collaborate:
- In pairs, make each other collaborators on a repo
- Add funny comment to the other file, commit, push it
- View version history/commits on github site

####Assessment

- Has created github account
- Able to run git command line tools
- Has at least one repo with two commits/changes
- Is collaborator on another repo, has changes someone elses repo.
- Probe class for basic git commants: What's the commant for saving a change in my local repository? How do I get a new version of a file on github? How do I find file status?









