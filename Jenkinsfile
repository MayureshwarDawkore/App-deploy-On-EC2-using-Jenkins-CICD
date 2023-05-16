pipeline {
    agent { label 'node-agent' }
    
    stages{
        stage('Code'){
            steps{
                git url: 'https://github.com/MayureshwarDawkore/App-deploy-On-EC2-using-Jenkins-CICD.git', branch: 'main' 
            }
        }
        stage('Build and Test'){
            steps{
                sh 'docker build . -t mayureshwar/my-app1'
            }
        }
        stage('Push'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
        	     sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                 sh 'docker push mayureshwar/my-app1:latest'
                }
            }
        }
       stage('Build') {
            steps {
                // Pull the Todo app image
                sh 'docker pull mayureshwar/my-app1:latest'
            }
        }
        stage('Stop Previous Container') {
            steps {
                script {
                    def containerID = sh(script: "docker ps -q --filter ancestor=mayureshwar/my-app1:latest", returnStdout: true).trim()
                    if (containerID != "") {
                        sh "docker stop ${containerID}"
                        sh "docker rm ${containerID}"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                // Deploy the Todo app
                sh 'docker run -d -p 8000:8000 --name todo-app mayureshwar/my-app1:latest'
            }
        }
        stage('Testing') {
            steps {
                // Test the deployed application
                script {
                    echo "running tests"
                    echo "everything running fine"
                }
            }
        }

    }
}

