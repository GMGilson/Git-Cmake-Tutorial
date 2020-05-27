1. Initialize repo
	~$ git init
		Server repository with:
			--bare 
			$ git remote add server user@hostname:/path/to/bare/directory
			

2. Add files to repo
	~$ git add {filename}

3. Commit changes to repo
   Will open default for you to commit with message (not optional)
	~$ git commit (with --all {-a} tag commit all staged changes)
	if using vim type 'a' to begin editing
	when done editing type "ctrl+c' to break edit and then use ':wq' to exit and save vim

4. Check status of repo
	~$ git status
		returns:
			files staged to be changed
			files not included in repo

~$ git checkout {filename} {branch}
	reverts uncommitted changes to the modified file if branch no specified
	brings filename from branch into current branch as changes
	moves HEAD to branch if filename unspecified
				
Adding files to a .gitignore file will make file not added to the repo not be flagged by git status
	Files can be exculed using;
		Unix wildcards:: *.txt (all files endingin .txt)
		Directories :: {directory_name}/
		Files :: placeholder.exe
		Exception to pattern ! :: !something.txt (even though files ending in .txt are ignored something.txt is tracked)

~git clean 
	-d 
		remove untracked directories aswell as untracked files

	-f --force 
		will delete untracked files 
		
	-i --interactive
		interactively clean git repo
	
	-e {pattern}, --exclude {pattern}
		exclude pattern from removal
	

Use ~$ git diff
	to see unstaged changes compared to commited files in repo

~$ git rm [--cached (stops git from tracking but doesnt rm from disk)] {file}
	stages file to be removed

~$ git log [--patch {-p} shows changes between commits] [-123 shows 123 patches starting from last commit]

~$ git commit --amend 
	commits staged changes to last commit (incase you forgot something)

~$ git remote (displays online branches) {--verbose returns more information on branche}
	add - [short name] [url/ssh] adds online repo to push changes to:: this is usually where meaningful changes are made
	remove - [short name] removes short address of online repo


~$ git push {remote repo} {branch you want to push}
	pushes changes to online repo for others to clone or fetch	
	using {--delete} before the branch you want to push will delete the branch locally 

~$ git fetch {remote branch name} (-all) 
	pulls remote branches to local to be checkout out
	must explictly checkout fetched branches like local branches	

~$ git tag [--annotated -a (without -a it appears as a lightweight tag)] 
	creates flags/comments on commits to denote important versions

~$ git show [tagname] 
	returns git log of that commit

~$ git branch newBranchName
	creates a new branch with copies of original files
	doesnot switch to the branch
	using the {-d} tag followed by a branch name will delete that branch 
	using the {-D] tag forcefully deletes the brach that follows the directive
		(these deleted branches are local changes that must be committed)
	

~$ git checkout BranchName
	switches to BranchName

~$ git checkout BranchName <pathtoFile>
	merges specific files to current branch

~$ git merge ${branch}
	merges ${branch} to the currently checked out branch
	if conflict between branches will show diff between branches and ask with changes you want to keep

~$ git rebase ${branch} #ACTION dependent on working branch 
	#define feature = non master branch
	#define release = master branch or branch that does not recieve many updates
	Case: bringing release changes to feature branch
		will advance feature branch to latest release commit and append any changes made in feature

	Case: bringing feature changes to release branch
		will bring all feature commits from last identical commit to release

	Visualize:

	master  : m1 - m2 - m3
		 		   |
	feature :      m2 - f1 - f2

	We want to move f1 and f2 commits to m3 without overriding the m3 commit

	(in feature)
	git rebase master

	master  : m1 - m2 - m3
		 		        |
	feature :      		m3 - f1 - f2

	now we can either merge to master 
	or
	(in master)
	git rebase feature

	master  : m1 - m2 - m3 - f1 - f2
		 		   		|
	feature :      		m3 - f1 - f2