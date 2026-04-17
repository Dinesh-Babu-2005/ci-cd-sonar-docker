pipeline {
    agent any

    tools {
        maven 'Maven3' // Change this if your tool name in Jenkins is different
    }

    environment {
        DOCKER_IMAGE = "ex5-app"
    }

    stages {

        stage('Checkout') {
            steps {
                // Don't forget to put your actual URL here before running!
                git branch: 'main', url: 'YOUR_GIT_REPO_URL' 
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    bat 'mvn sonar:sonar'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t ex5-app .'
            }
        }

        stage('Run Container') {
            steps {
                // The '|| echo...' prevents the pipeline from crashing on the first run
                bat 'docker rm -f ex5-container || echo "No container to remove"'
                bat 'docker run -d --name ex5-container -p 8080:8080 ex5-app'
            }
        }
    }
}