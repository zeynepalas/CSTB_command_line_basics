
# Introduction to Git and GitHub

## Why Use Git?

Git is a powerful tool for version control that helps developers manage and track changes in their code. It is widely used in both personal projects and collaborative environments. Here are the main reasons to use Git:

- **Version Control for Your Code:** Git allows you to track changes in your code, revert back to previous versions, and maintain an organized history of your project.
- **Track Changes in Your Code's History:** With Git, every change you make can be logged, and you can compare versions to see what has been modified over time.
- **Collaborate with Others:** Git enables multiple developers to work on the same codebase simultaneously without conflicts. By managing changes through branches and merges, Git keeps everything organized.
- **Backup Your Code Remotely:** Using Git hosting platforms like GitHub or GitLab, you can store your code remotely. This acts as a backup, ensuring that your work is safe even if your local machine fails.

## Git Hosting Services:

Git repositories need a remote service to be easily accessible, particularly when working collaboratively. Two popular Git hosting services are:

- **GitHub:** GitHub is a web-based platform that provides Git repository hosting. It also offers additional features such as issue tracking, code review, continuous integration, and collaboration tools.
- **GitLab:** GitLab is another Git hosting platform that offers similar features as GitHub. It includes Git repositories, continuous integration, and a variety of tools for managing your code and project lifecycle.

## Connect Your Local Git to SSH

Using SSH (Secure Shell) keys is a secure method of connecting to Git hosting services like GitHub or GitLab. This allows you to push and pull changes from your repositories without needing to enter your username and password each time.

### Connect in command line

To set up SSH:

1. Generate an SSH key on your local machine:
   
```bash
ssh-keygen -t rsa -b 4096
```

2. Add the SSH key to your Git hosting service account (GitHub or GitLab). Go to your account settings -> SSH Keys, and add the public key (usually found in `~/.ssh/id_rsa.pub`).
3. Test the SSH connection:

```bash	
ssh -T git@github.com
```

### Connect with VSCode

You can also connect your local Git to GiHub using the VSCode interface.

1. Open VSCode and go to the Source Control tab.
2. Click on the gear icon and select "Clone Repository."
3. Choose the repository you want to clone and select "Clone."
4. You will be prompted to sign in to your GitHub account and authorize the connection.
5. Once connected, you can push and pull changes directly from VSCode.
   
**Exercise**: Connect your GitHub account to your local machine (in you case: your server account) using the method of your choice.

## Cloning a Repository

Cloning a repository means creating a copy of a remote repository on your local machine. To clone a repository, use the following command:

```bash
git clone <repository_url>
```

## Forking a Repository

Forking is the process of creating a copy of someone else's repository under your account. This is useful if you want to make changes to a project but don't have direct write access to the original repository. You can later submit a pull request to propose your changes.

To fork a repository:
1. Go to the repository page on GitHub or GitLab.
2. Click the "Fork" button to create a copy of the repository under your own account.

**Exercise**: Create a folk of this repository and clone it to your local machine.

## The Commit

A commit is the fundamental unit of change in Git. It represents a snapshot of the project at a given point in time. A commit includes:

- **A set of changes** (the diff, which shows what files were modified and how).
- **A commit message**, which describes the changes you made.

Each commit is identified by a unique hash (a long string of characters) that serves as a reference to that specific snapshot.

Example: 

```bash
git commit -m "First release of Hello World!"
```

Output:

```
[master (root-commit) 221ec6e] First release of Hello World!
 3 files changed, 26 insertions(+)
 create mode 100644 README.md
 create mode 100644 bluestyle.css
 create mode 100644 index.html
```

The identifier `221ec6e` is the commit hash, the message `add git.md` describes the changes made in this commit and the next lines shows the files that were created, modified or removed and how many lines were added or deleted.

### Best Practices for Commit

- **1 commit = 1 feature:** It is best practice to commit related changes together. For example, don't commit code that changes the layout of a website alongside code that adds a new feature.
- **Descriptive commit messages:** Make sure your commit messages clearly describe what the changes are. A good commit message helps you and others understand what was done and why.

#Comment: add pictures

## Diff

A diff is a comparison between two versions of the code. It shows the changes made between those versions, highlighting what was added, modified, or deleted.

### Using the Command Line

```bash
git diff <commit1> <commit2>
```

### Using VSCode

#Comment: look if it stay

### On GitHub
#Comment: look if it stay

## Information Commands

Git provides several commands to get information about your project and repository:

- Show the **status** of your working directory: `git status`
- View the **commit history**: `git log`
- Show the differences between commits: `git diff <commit1> <commit2>`
  
## Adding or Removing Files

To track new files or remove files from Git, use the following commands:

- **Add new or modified files** to the staging area: `git add <file_name>` (`git add .` to add all files)    
- **Remove files** from Git's tracking: `git rm <file_name>`
  
## Committing Changes

Once you've added or removed files, you can commit the changes to Git with the following command:

```bash
git commit -m "Your commit message here"
```

## Pushing and Pulling Changes Remotely

Git allows you to synchronize your local repository with a remote repository, such as GitHub or GitLab. 

- Push your commits to the remote repository: `git push origin <branch_name>`
- Pull changes from the remote repository: `git pull origin <branch_name>`

These commands ensure that your local repository is in sync with the remote repository, allowing for collaboration.

## Branches

Branches are a way to work on different versions of your project. Using branches helps keep your main (master) branch stable while you work on new features or fixes.

### Why Use Branches?

- **Collaborative work:** Multiple people can work on different branches without interfering with each other’s work.
- **Feature development:** You can create a branch for each new feature or bug fix, keeping your main branch stable and functional.


### Working with Branches

- Create a new branch: `git branch <branch_name>`
- Switch to a branch: `git checkout <branch_name>`
- Create and switch to a branch in one command: `git checkout -b <new_branch>`
- Delete a branch: `git branch -d <branch_name>`
- List all branches: `git branch`
  
### Merging Branches

Once you’ve completed your work on a branch and are ready to integrate the changes into the main branch, you can merge the branch:

1. Switch to the main branch: `git checkout main`
2. Merge the other branch into the main branch: `git merge <branch_name>`

## References

- [Formation Git: Ou comment bien collaborer à plusieurs sur son code](https://viarezo.fr/assets/files/formations/2022_Git_slides.pdf)
