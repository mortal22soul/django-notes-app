pipeline{
    agent {label "vinod"}
    
    stages{
        stage("code"){
            steps{
                echo "this is cloning the code"
                git branch: 'dev', url: 'https://github.com/LondheShubham153/django-notes-app.git'
                echo "code cloned"
            }
        }
        stage("build"){
            steps{
                echo "this is building the code"
                sh "docker build -t notes-app:latest ."
                echo "build completed"
            }
        }
        stage("pushing"){
            steps{
                echo "this is pushing image to docker hub"
                withCredentials([usernamePassword(
                    "credentialsId":"dockerhubcred", 
                    passwordVariable:"dockerHubPass", 
                    usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                    sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage("test"){
            steps{
                echo "this is testing the code"
            }
        }
        stage("deploy"){
            steps{
                echo "this is deploying the code"
                sh "docker-compose down && docker-compose up -d"
                echo "app deployed on port 8000"
            }
        }
    }
}
