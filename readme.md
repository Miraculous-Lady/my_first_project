GitHub version control 
A. Clone an existing GitHub repository
1. Create GitHub account → Create repository with README.

2. Install Git Bash → Open VS Code → Open Terminal.

3. Check Git installation
git --version


4. Configure Git (first time only)
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --list


5. Clone repository → GitHub → Green Code button → HTTPS → Copy URL.
git clone PASTE_URL
cd REPOSITORY_NAME


6. Check files and status
git status
ls
ls -a
pwd
clear


7. Edit README.md in VS Code → Save → Check changes
git status


8. Add → Commit → Push
git add .
git commit -m "Updated README"
git push origin main


9. Pull updates from GitHub
git pull origin main


B. Upload a new local repository to GitHub
1. Create folder → Enter folder
mkdir localrepo
cd localrepo


2. Initialize Git
git init


3. Create index.html and style.css in VS Code and save.

4. Add and commit files
git add .
git commit -m "Add initial files"


5. Create a new GitHub repository without README → Copy HTTPS URL.

6. Connect local repository
git remote add origin PASTE_URL
git remote -v


7. Rename branch and upload
git branch
git branch -M main
git push -u origin main

