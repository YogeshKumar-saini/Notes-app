pipeline {
    agent any 
    
    stages {  
        
        stage("Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/YogeshKumar-saini/Notes-app", branch: "main"
            }
        }

        stage("Build") {
            steps {
                echo "Building the code"
                sh "docker build -t 22bcd0052-notes-app ."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                echo "Pushing image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub1", passwordVariable: "dockerHubPassword", usernameVariable: "dockerhubUser")]) {
                    echo "DockerHub Username: ${env.dockerhubUser}" // Debugging Output
                    sh "docker tag 22bcd0052-notes-app ${env.dockerhubUser}/22bcd0052-notes-app:latest"
                    sh "echo '${env.dockerHubPassword}' | docker login -u '${env.dockerhubUser}' --password-stdin"
                    sh "docker push ${env.dockerhubUser}/22bcd0052-notes-app:latest"
                }
            }
        }

        stage("Deploy the code") { 
            steps {
                echo "Deploying the code"
                echo "Trying to run container with image: yogesh1090/22bcd0052-notes-app:latest" // Debugging Output
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
