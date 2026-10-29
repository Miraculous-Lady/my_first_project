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


GitHub Actions —----------------------------------------------------------------------------------------------------------------------
Aim: To automate application deployment using GitHub Actions.
1. Create and clone repository
Create GitHub repository automated-deployment, then copy its HTTPS URL.
git clone REPOSITORY_URL
cd automated-deployment


2. Create application
Create index.html and add HTML content:
<h1>Hello from GitHub Actions!</h1>
<p>This application is deployed automatically.</p>


Push the file:
git add .
git commit -m "Add application"
git push origin main


3. Create workflow file
Inside the project, create:

.github/workflows/deploy.yml

Add this code:
name: Deploy Application

on:
  push:
    branches:
      - main

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/configure-pages@v5
      - uses: actions/upload-pages-artifact@v3
        with:
          path: '.'
      - uses: actions/deploy-pages@v4


4. Push workflow to GitHub
git add .github/workflows/deploy.yml
git commit -m "Add deployment workflow"
git push origin main


5. Enable GitHub Pages
GitHub repository → Settings → Pages → Build and deployment → Source → GitHub Actions.
6. Check deployment
Repository → Actions → Open workflow → Wait for green check mark.

Visit:

https://USERNAME.github.io/REPOSITORY-NAME/

Replace the placeholders with your GitHub username and repository name.
Remember the flow
Create repo → Add HTML → Create workflow → Push → Enable Pages → Check Actions → Open website

Result: Application deployment is automated using GitHub Actions whenever changes are pushed to the main branch.


Kubernetes —-------------------------------------------------------------------------------------------------------------------------

Aim: To deploy and manage containers using Kubernetes.

### 1. Check installation

```
docker --version
kubectl version --client
kubectl get nodes
```

Node status should be `Ready`. Enable Kubernetes in Docker Desktop first.

### 2. Create application

Create `app.py`:

```
from flask import Flaskapp = Flask(__name__)@app.route("/")def home():    return "Hello from Kubernetes!"app.run(host="0.0.0.0", port=5000)
```

Create `requirements.txt`:

```
flask
```

### 3. Create Dockerfile

```
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app.py .
EXPOSE 5000
CMD ["python", "app.py"]
```

### 4. Build and upload image

Replace `YOUR_USERNAME` with your Docker Hub username.

```
docker build -t my-flask-app:1.0 .
docker login
docker tag my-flask-app:1.0 YOUR_USERNAME/my-flask-app:1.0
docker push YOUR_USERNAME/my-flask-app:1.0
```

### 5. Create Deployment

Create `deployment.yaml`:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: flask-app
  template:
    metadata:
      labels:
        app: flask-app
    spec:
      containers:
        - name: flask-container
          image: YOUR_USERNAME/my-flask-app:1.0
          ports:
            - containerPort: 5000
```

Apply and check:

```
kubectl apply -f deployment.yaml
kubectl get deployments
kubectl get pods
```

### 6. Expose application using Service

Create `service.yaml`:

```
apiVersion: v1
kind: Service
metadata:
  name: flask-service
spec:
  selector:
    app: flask-app
  ports:
    - port: 80
      targetPort: 5000
  type: NodePort
```

```
kubectl apply -f service.yaml
kubectl get services
kubectl port-forward service/flask-service 8080:80
```

Open `http://localhost:8080` in your browser.

### 7. Scale Pods

```
kubectl scale deployment flask-deployment --replicas=5
kubectl get pods
```

Reduce to two:

```
kubectl scale deployment flask-deployment --replicas=2
```

### 8. Update and rollback

```
docker build -t YOUR_USERNAME/my-flask-app:2.0 .
docker push YOUR_USERNAME/my-flask-app:2.0
kubectl set image deployment/flask-deployment flask-container=YOUR_USERNAME/my-flask-app:2.0
kubectl rollout status deployment/flask-deployment
```

Rollback if the update fails:

```
kubectl rollout undo deployment/flask-deployment
```

### 9. Monitor and troubleshoot

```
kubectl get pods
kubectl get all
kubectl describe pod POD_NAME
kubectl logs POD_NAME
kubectl get pods -w
kubectl rollout restart deployment/flask-deployment
```

Replace `POD_NAME` with the actual Pod name.

### 10. Delete resources

```
kubectl delete -f service.yaml
kubectl delete -f deployment.yaml
```

## Quick revision

| Term / Command         | Meaning                             |
| ---------------------- | ----------------------------------- |
| Docker image           | Application package                 |
| Pod                    | Smallest deployable Kubernetes unit |
| Deployment             | Manages Pods and replicas           |
| Service                | Provides stable network access      |
| `kubectl apply -f`     | Create/update resources             |
| `kubectl get pods`     | View Pods                           |
| `kubectl scale`        | Change replica count                |
| `kubectl logs`         | View application logs               |
| `kubectl rollout undo` | Roll back an update                 |

Remember the workflow:

Dockerfile → Build Image → Docker Hub → Deployment → Pods → Service → Application

Result: The application is deployed and managed using Kubernetes, including scaling, updating, monitoring, and rollback.

Apache Airflow —------------------------------------------------------------------------------------------------------------------------
Aim: Design, schedule, execute and monitor a workflow using Apache Airflow with Docker.

1. Create folders (inside airflow-project)
mkdir dags
mkdir logs
mkdir plugins
mkdir config


Download the official docker-compose.yaml from Apache Airflow documentation.

2. Start Airflow
docker compose up airflow-init
docker compose up -d
docker compose ps


Open http://localhost:8080 and log in using the configured credentials.

3. Create DAG — dags/simple_workflow.py
from datetime import datetimefrom airflow import DAGfrom airflow.operators.python import PythonOperatordef task1():    print("Hello! Airflow is running.")def task2():    print("Processing data...")def task3():    print("Workflow completed!")with DAG(    dag_id="simple_workflow",    start_date=datetime(2026, 9, 22),    schedule="@daily",    catchup=False) as dag:    a = PythonOperator(task_id="print_message", python_callable=task1)    b = PythonOperator(task_id="process_data", python_callable=task2)    c = PythonOperator(task_id="finish_workflow", python_callable=task3)    a >> b >> c



4. Execute and monitor
- Refresh Airflow UI and find simple_workflow.
- Turn the DAG ON and click Trigger DAG.
- Open Graph/Grid View to check task status.
- Click a task → Logs to view output.
5. Useful commands
docker compose ps
docker compose logs
docker compose logs airflow-scheduler
docker compose down
docker compose up -d


Quick revision
- DAG: Defines workflow and task dependencies.
- Task: Individual unit of work.
- a >> b >> c: Executes tasks in sequence.
- @daily: Schedules a daily run.
- catchup=False: Prevents automatic runs for past scheduled dates.
- Logs: Show task output and errors.
- Docker Compose: Runs and manages Airflow containers.
Workflow: Docker → Airflow → DAG → Tasks → Scheduling → Execution → Monitoring.

Result: A scheduled workflow was created, executed and monitored using Apache Airflow and Docker.
