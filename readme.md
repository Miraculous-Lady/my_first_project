GitHub version control -------------------------------------------------------------------------------------------------------
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


Jenkins Build Pipeline --------------------------------------------------------------------------------------------
Aim: To set up a build pipeline using Jenkins.
1. Install and start Jenkins
- Download jenkins.war from Jenkins.io.
- Open CMD/Terminal and go to Downloads:
cd Downloads
java -jar jenkins.war


- Change port if required:
java -jar jenkins.war --httpPort=9191


- Open browser: http://localhost:8080
2. Create 4 Jenkins jobs
Dashboard → New Item → Enter job name → Select Freestyle project → OK.

Under Build Steps → Execute shell, enter:
echo "Build process successfully done"


Click Apply → Save.

Repeat for 4 jobs: Build, Test, Deploy, Release.
3. Link jobs in sequence
For each next job:

Configure → Build Triggers → Build after other projects are built

Enter the previous job's name → Apply → Save.

Link sequence:

Build → Test → Deploy → Release

Run the first job; the next jobs run automatically when configured correctly.
4. Install Build Pipeline plugin
Manage Jenkins → Plugins → Available plugins → Search Build Pipeline → Install.
5. Create pipeline view
Dashboard → + (Add View) → Enter view name → Select Build Pipeline View → OK.
- Pipeline Flow: Select initial job (Build).
- Display Options: Set number of displayed builds.
- Click Apply → Save.
Remember
Install → Start → Create 4 Jobs → Link Jobs → Install Plugin → Create Pipeline View

Result: Jenkins build pipeline is configured to execute jobs sequentially.

Docker --------------------------------------------------------------------------------------------------------------------------------
Aim: To containerize and run an application using Docker.
1. Install Docker
- Download and install Docker Desktop.
- Enable WSL 2 if required and open Docker Desktop.
- Verify installation in PowerShell:
docker --version


2. Create application
Create folder myapp → Create app.py inside it:
print("Hello from Docker!")



3. Create Dockerfile
Inside myapp, create a file named Dockerfile (no extension):
FROM python:3.10-slim
WORKDIR /app
COPY . /app
CMD ["python", "app.py"]


4. Build Docker image
Open terminal inside the myapp folder:
docker build -t myapp-image .


5. Run container
docker run --name myapp-container myapp-image


Output: Hello from Docker!
6. Verify containers
docker ps -a


7. Stop and remove (optional)
docker stop myapp-container
docker rm myapp-container


Note: This app finishes immediately, so the container exits automatically. docker ps -a will still show it.
Remember the sequence
Install → Create app → Dockerfile → Build → Run → Verify
- docker build = Create image
- docker run = Run container
- docker ps -a = Show all containers

Docker Compose —------------------------------------------------------------------------------------------------------------------
Aim: To create and run Docker containers using Docker Compose.
1. Dockerfile vs Docker Compose
- Dockerfile = Instructions to build an image.
- Docker Compose = YAML file to configure and run services/containers.
- Nginx = Web server used to serve web pages.
2. Create compose.yml
For a simple Nginx web server, create compose.yml:
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"


3. Run the container
docker compose up -d


4. Check running services
docker compose ps


5. Open browser
http://localhost:8080

The Nginx welcome page should appear.
6. Stop and remove containers
docker compose down



Multi-service application (Web + Backend + Database)
Create compose.yml:
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    depends_on:
      - app

  app:
    image: node:20-alpine
    working_dir: /app
    volumes:
      - ./app:/app
    command: node server.js
    depends_on:
      - db

  db:
    image: postgres:16
    environment:
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypassword
      POSTGRES_DB: mydatabase


Run and check:
docker compose up -d
docker compose ps
docker compose down


Note: This multi-service example requires the backend files (including app/server.js) to exist. The Nginx example is the simplest one to demonstrate in a practical exam.
Remember
Create YAML → up → ps → Test in browser → down
- up -d = Start services in the background.
- ps = Check services.
- down = Stop and remove services.
