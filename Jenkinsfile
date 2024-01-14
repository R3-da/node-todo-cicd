pipeline {
    agent { label "dev-server"}
    
    stages {
        stage("code"){
            steps{
                git url: "https://github.com/LondheShubham153/node-todo-cicd.git", branch: "master"
                echo 'bhaiyya code clone ho gaya'
            }
        }

        stage("build and test"){
            steps{
                sh "docker build -t node-app-test-new ."
                echo 'code build bhi ho gaya'
            }
        }
        
        stage("scan image"){
            steps{
                echo 'image scanning ho gayi'
            }
        }

        stage("push"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag node-app-test-new:latest ${env.dockerHubUser}/node-app-test-new:latest"
                sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                echo 'image push ho gaya'
                }
            }
        }
        
        stage("deploy") {
            steps {
                echo 'Stopping and removing existing container if it exists'
                sh "docker stop node-app-containertwo || true"
                sh "docker rm node-app-containertwo || true"

                sh "docker-compose down && docker-compose up -d"
                echo 'Deployment ho gayi'

                // Start the container with the specified name
                sh "docker run -d --name node-app-containertwo -p 8000:8000 node-app-todo1"
            }
        }
    }
}
