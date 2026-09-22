# Team B - Flask Hello World

A simple Flask Hello World application that Team B will use for our Dockerization project.

## Run Locally

Clone the repository:

```bash
git clone https://github.com/dejinassar/team-b-dockerized-flask.git
cd team-b-dockerized-flask
```

Create and activate a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Run the application:

```bash
export FLASK_APP=main.py
flask run
```

Open:

```
http://127.0.0.1:5000
```

You should see:

```
Hello world with Flask
```

## Next Step

The next stage of the project is to Dockerize this application.
