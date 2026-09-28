# Team B – Dockerized Flask Hello World

A simple Python Flask "Hello World" web application, packaged with Docker so it runs the same way on any machine.

**Repository:** https://github.com/dejinassar/team-b-dockerized-flask

---

## 1. Project Overview

This project takes a minimal Flask web application and containerizes it with Docker. The app has one job: when you open it in a browser, it displays:

```
Hello world with Flask
```

The application was built and tested with Docker on Ubuntu. After building the image and starting the container, the browser successfully displayed the message above.

The Docker setup lives on the **`feature/dockerize-app`** branch.

## 2. Objective

The goal of this project is to demonstrate the basic Docker workflow:

```
Application → Dockerfile → Docker Image → Docker Container → Running Web Application → Reproducibility
```

In plain terms: we write the app, describe how to package it in a Dockerfile, build an image, run that image as a container, and confirm that any teammate can reproduce the same result.

## 3. Technologies Used

| Technology | Details |
|---|---|
| Python | 3.13 (via the `python:3.13-slim` base image) |
| Flask | 3.1.2 |
| Docker | Image building and container runtime |
| Git & GitHub | Version control and collaboration |
| Ubuntu | Environment where the Docker setup was tested |

## 4. Project Structure

```
team-b-dockerized-flask/
├── main.py             # Flask application (Hello World)
├── requirements.txt    # Python dependencies (Flask 3.1.2)
├── Dockerfile          # Instructions for building the Docker image
└── README.md           # Project documentation
```

## 5. Running the Application Without Docker

You can run the app directly on your machine first, to confirm it works before using Docker.

**Requirements:** Python 3.13 and pip.

```bash
# Create and activate a virtual environment
python3 -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Start the app
flask --app main.py run --port 5000
```

Open your browser at http://localhost:5000. You should see **Hello world with Flask**.

Press `Ctrl + C` in the terminal to stop the app.

## 6. Dockerfile Explanation

```dockerfile
FROM python:3.13-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["flask", "--app", "main.py", "run", "--host=0.0.0.0", "--port=5000"]
```

| Instruction | What it does |
|---|---|
| `FROM python:3.13-slim` | Starts from a small official Python 3.13 image. |
| `WORKDIR /app` | Sets `/app` as the working directory inside the container. |
| `COPY requirements.txt .` | Copies the dependency list first, so Docker can cache the install step. |
| `RUN pip install --no-cache-dir -r requirements.txt` | Installs Flask and other dependencies without keeping the pip cache, which keeps the image smaller. |
| `COPY . .` | Copies the rest of the project files into `/app`. |
| `EXPOSE 5000` | Documents that the app listens on port 5000. |
| `CMD [...]` | Starts the Flask app when the container runs. `--host=0.0.0.0` makes it reachable from outside the container. |

## 7. Building the Docker Image

From the project folder (where the `Dockerfile` is), run:

```bash
docker build -t team-b-flask .
```

- `-t team-b-flask` gives the image the name `team-b-flask`.
- The `.` tells Docker to use the current folder as the build context.

## 8. Running the Docker Container

```bash
docker run -d -p 5000:5000 --name team-b-app team-b-flask
```

- `-d` runs the container in the background (detached mode).
- `-p 5000:5000` maps port 5000 on your machine to port 5000 in the container.
- `--name team-b-app` names the container `team-b-app`.
- `team-b-flask` is the image to run.

## 9. Testing the Application

Open a browser and go to:

```
http://localhost:5000
```

Expected output:

```
Hello world with Flask
```

You can also test from the terminal:

```bash
curl http://localhost:5000
```

## 10. Checking Container Status and Logs

**List running containers:**

```bash
docker ps
```

You should see `team-b-app` in the list with port `5000` mapped.

**View the container logs:**

```bash
docker logs team-b-app
```

**Stop and remove the container when you are done:**

```bash
docker stop team-b-app
docker rm team-b-app
```

## 11. How Another Team Member Can Clone and Run It

**Step 1 – Clone the repository:**

```bash
git clone https://github.com/dejinassar/team-b-dockerized-flask.git
cd team-b-dockerized-flask
```

**Step 2 – Switch to the Docker branch (if needed):**

The Docker implementation is on the `feature/dockerize-app` branch. If the files such as the `Dockerfile` are not visible after cloning, switch to that branch:

```bash
git checkout feature/dockerize-app
```

To confirm which branch you are on:

```bash
git branch
```

> If `git checkout` does not find the branch, run `git fetch` first and try again.

**Step 3 – Build the image:**

```bash
docker build -t team-b-flask .
```

**Step 4 – Run the container:**

```bash
docker run -d -p 5000:5000 --name team-b-app team-b-flask
```

**Step 5 – Open the app:**

Go to http://localhost:5000 and confirm you see **Hello world with Flask**.

## 12. Troubleshooting

| Problem | Possible cause | Fix |
|---|---|---|
| `Dockerfile` not found when building | You are on the wrong branch or in the wrong folder. | Run `git checkout feature/dockerize-app` and make sure you are inside `team-b-dockerized-flask`. |
| `port is already allocated` or `address already in use` | Something else is using port 5000. | Stop the other process, or run on a different host port, e.g. `docker run -d -p 5001:5000 --name team-b-app team-b-flask`, then open http://localhost:5001. |
| `container name "/team-b-app" is already in use` | A container with that name already exists. | Run `docker stop team-b-app` and `docker rm team-b-app`, then run it again. |
| Page does not load in the browser | Container is not running or crashed. | Run `docker ps` to check status, and `docker logs team-b-app` to see errors. |
| `permission denied` when running Docker on Ubuntu | Your user is not allowed to use Docker without `sudo`. | Use `sudo docker ...`, or add your user to the `docker` group and log in again. |
| Changes to the code do not show up | The image still contains the old code. | Stop and remove the container, rebuild with `docker build -t team-b-flask .`, and run it again. |

## 13. Conclusion

This project shows the complete Docker workflow for a simple web application: a Flask app is described by a Dockerfile, built into an image, and run as a container that serves **Hello world with Flask** on port 5000. Because everything the app needs is defined in the repository, any teammate can clone it, build the image, and get the same working result. That is the core benefit of Docker: **reproducibility**.

