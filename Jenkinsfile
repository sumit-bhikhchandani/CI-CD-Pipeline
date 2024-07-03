pipeline {
    agent any
    
    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the Code"
                git url: "https://github.com/sumit-bhikhchandani/To-dolist-for-CI-CD.git", branch: "main"
            }
        }
        
        stage("SonarQube Analysis") {
            steps {
                script {
                    echo "Performing SonarQube analysis."
                    def scannerHome = tool name: 'SonarQube Scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        
        stage("Build Docker Image") {
            steps {
                echo "Building the Docker image."
                sh "docker build -t todo-app ."
            }
        }
        
        stage("Deploy Docker Container") {
            steps {
                echo "Stopping and removing any existing container named 'todo-app'."
                sh "docker stop todo-app || true"
                sh "docker rm todo-app || true"
                
                echo "Running the Docker container."
                sh "docker run -d --name todo-app -p 8000:8000 todo-app"
            }
        }
    }
}
