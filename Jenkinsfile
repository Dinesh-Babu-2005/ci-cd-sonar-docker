pipeline {
    agent any

    tools {
        maven 'Maven3' // Make sure this matches your Global Tool Configuration name
    }

    environment {
        DOCKER_IMAGE = "ex5-app"
    }

    stages {

        stage('Checkout') {
            steps {
                // Your actual GitHub repo URL is here now
                git branch: 'main', url: 'https://github.com/Dinesh-Babu-2005/ci-cd-sonar-docker' 
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
                bat 'docker rm -f ex5-container || echo "No container to remove"'
                bat 'docker run -d --name ex5-container -p 8081:8080 ex5-app'
            }
        }
    }
}