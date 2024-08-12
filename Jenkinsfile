pipeline {
    agent any
    
    stages{
        stage("Cloning Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/RupaliKhare/django-notes-app.git", branch: "main"
            }
        }
        
        stage("Code Build"){
            steps {
                echo "building code"
                sh "docker build . -t mynotesapp"
            }
        }
        
        stage("push to docker hub"){
            steps {
                echo "deploy to docker hub"
                withCredentials(
                    [
                        usernamePassword(
                            credentialsId:"dockerhub-login",passwordVariable:"dockerhubPass",usernameVariable:"dockerhubuser")]){
                                sh "docker tag mynotesapp ${env.dockerhubuser}/mynotesapp:latest"
                                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubPass}"
                                sh "docker push ${env.dockerhubuser}/mynotesapp:latest"
                    }
            }
        }
        
        stage('Install Docker Compose and Deploy') {
            steps {
                sh 'curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o $HOME/docker-compose'
                sh 'chmod +x $HOME/docker-compose'
                sh '$HOME/docker-compose --version'
                sh 'export PATH=$PATH:$HOME && $HOME/docker-compose down && $HOME/docker-compose up -d'
            }
        }
    }
}
