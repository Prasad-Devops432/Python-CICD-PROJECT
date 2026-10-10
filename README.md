# Python Flask App: Jenkins CI/CD

A small Flask web application with three pages (`/`, `/about`, and a `/health`
JSON endpoint) and a Jenkins pipeline that tests, builds and deploys it as a
Docker container.

## Project structure

```
.
├── app.py                 # Flask application (routes: /, /about, /health)
├── templates/
│   ├── index.html         # Home page
│   └── about.html         # About page
├── test_app.py            # pytest tests
├── requirements.txt       # Python dependencies (pinned versions)
├── Dockerfile             # Container image, served by gunicorn
├── Jenkinsfile            # CI/CD pipeline
└── .gitignore
```

## Run locally

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
python app.py
```

Then open http://localhost:5000. The health endpoint is at http://localhost:5000/health.

## Run the tests

```bash
pytest test_app.py -v
```

## Run with Docker

```bash
docker build -t python-flask-app:local .
docker run --rm -p 5000:5000 python-flask-app:local
```

## Jenkins pipeline

The pipeline is defined in `Jenkinsfile`:

1. **Checkout:** clones the repository that triggered the build.
2. **Setup Python environment:** creates a virtual environment and installs `requirements.txt`.
3. **Code quality check:** runs `flake8` on `app.py`.
4. **Run unit tests:** runs `pytest` with coverage.
5. **Build Docker image:** tags the image with the build number and `latest`.
6. **Deploy:** replaces any running container and starts the new one on port 5000.

On failure, the pipeline stops and removes the container it started.

## Notes

- The Dockerfile uses `python:3.9-slim`. Check that the pinned versions in `requirements.txt` install on that Python version before relying on the image.
- The deploy stage runs on the Jenkins agent's own Docker daemon, so the app is reachable on that machine, not on a cloud server.
- The app has no database or authentication.
