# Python Flask App — Jenkins CI/CD

A simple Python Flask web application with full Jenkins CI/CD pipeline.

## Project Structure

```
python-jenkins-app/
├── app.py                  # Main Flask application
├── requirements.txt        # Python dependencies
├── Dockerfile              # Docker image build
├── Jenkinsfile             # Jenkins pipeline
├── .gitignore
├── templates/
│   ├── index.html          # Home page
│   └── about.html          # About page
└── tests/
    └── test_app.py         # Unit tests
```

## How to Run Locally

```bash
# Install dependencies
pip install -r requirements.txt

# Run app
python app.py
```

Visit http://localhost:5000

## Jenkins Pipeline Stages

1. Checkout Code
2. Setup Python Environment
3. Code Quality Check (flake8)
4. Run Unit Tests (pytest)
5. Build Docker Image
6. Deploy Application
7. Health Check

## Requirements

- Jenkins with Docker installed
- Python 3.9+
- Docker
