pipeline {
    agent any
    
    stages {
        stage("code"){
            steps{
                echo "code clone"
                git url:"https://github.com/salman6898/django-notes-app.git", branch:"main"
            }
        }
        stage("Build"){
            steps{
                echo "building the image"
                sh "docker build -t mynote-app ."
            }
        }
        stage("Push to dockerhub"){
            steps{
                echo "push to docker hub"
                withCredentials([usernamePassword(credentialsId:"DockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag mynote-app ${env.dockerHubUser}/my-note-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-note-app:latest"
                }
                
            }
        }
        stage("Deploy"){
            steps{
                echo "deploying the container"
                sh "docker-compose down && docker-compose up -d"
                
            }
        }
    }    
}
