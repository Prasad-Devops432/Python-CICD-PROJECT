pipeline {
    agent any

    environment {
        APP_NAME    = 'python-flask-app'
        DEPLOY_PORT = '5000'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Prasad-Devops432/Portfolio-Project-.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Code Quality Check') {
            steps {
                sh '''
                . venv/bin/activate
                flake8 app.py --max-line-length=100 --ignore=E501
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh '''
                . venv/bin/activate
                pytest tests/ -v --cov=app --cov-report=xml --cov-report=term
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${APP_NAME}:${BUILD_NUMBER} .
                docker tag ${APP_NAME}:${BUILD_NUMBER} ${APP_NAME}:latest
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                # Stop and remove old container if running
                docker stop ${APP_NAME} || true
                docker rm ${APP_NAME}  || true

                # Run new container
                docker run -d \
                    --name ${APP_NAME} \
                    -p ${DEPLOY_PORT}:5000 \
                    --restart always \
                    ${APP_NAME}:latest
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                sleep 5
                curl -f http://localhost:${DEPLOY_PORT}/health || exit 1
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment successful! App running at http://localhost:${DEPLOY_PORT}"
        }
        failure {
            echo "Pipeline failed. Check the logs above."
            sh '''
            docker stop ${APP_NAME} || true
            docker rm ${APP_NAME}   || true
            '''
        }
    }
}
