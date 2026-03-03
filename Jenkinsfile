pipeline{
    agent {label "worker"}
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/piyushadhalikar/django-notes-app.git", branch: "main"
                echo "code clone done"
            }
        }
        stage("build"){
            steps{
                sh "docker build -t notes-app:latest ."
            }
        }
        stage("test"){
            steps{
                echo "code testing done"
            }
        }
        stage("push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHubCred",usernameVariable:"dockerHubUser",passwordVariable:"dockerHubPass")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
                echo "code pushed to dockerhub"
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker compose down && docker compose up -d"
            }
        }
    }
}
