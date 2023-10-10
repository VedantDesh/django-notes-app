pipeline{
    agent any 
    
    stages{
        stage("Code"){
            steps{
                echo "Cloning the code"
                git url: "https://github.com/VedantDesh/django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
            steps{
                echo "Building the code"
                sh "docker build -t django-app-image ."
            }
        }
        stage("Push to Docker-Hub"){
            steps{
                echo "Pushing the code to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag django-app-image ${env.dockerHubUser}/django-app-image:latest "
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/django-app-image:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Code Deployment"
                sh "docker-compose down && docker-compose up -d"
            }
        }
        
    }
}
