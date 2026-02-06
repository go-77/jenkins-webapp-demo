pipeline {
    agent any

    environment {
        APP_NAME = "webapp-demo"
        APP_PORT = "8085"
    }

    triggers {
        githubPush()
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $APP_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker rm -f $APP_NAME || true
                docker run -d \
                    -p $APP_PORT:80 \
                    --name $APP_NAME \
                    $APP_NAME:$BUILD_NUMBER
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'curl -f http://localhost:$APP_PORT'
            }
        }
    }
}
