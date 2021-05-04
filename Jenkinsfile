pipeline {
    agent any

    stages {
        stage('CI') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'password', usernameVariable: 'username')]) {
                
                    sh """
                    docker build . -t leena1234/jenkins_test:1.0
                    docker login --username ${username} --password ${password}
                    docker push leena1234/jenkins_test:1.0
                    """
                }
            }
        }
        stage('CD') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'password', usernameVariable: 'username')]) {
                
                    sh """
                    docker run -d -p 8000:8000 leena1234/jenkins_test:1.0
                    """
                }
            }
            post {
                success {
                    slackSend (color: "#008000", message: "pipeline is done")
                }
            }
        }
    }
}