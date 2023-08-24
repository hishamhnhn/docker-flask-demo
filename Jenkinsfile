pipeline {
    agent any 
    environment {
    DOCKERHUB_CREDENTIALS = credentials('hishamkhalil')
    }
    stages { 
        stage('Build docker image') {
            steps {  
                sh 'docker build -t hishamkhalil/flaskapp:$BUILD_NUMBER .'
            }
        }
        stage('login to dockerhub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps {
                sh 'docker push hishamkhalil/flaskapp:$BUILD_NUMBER'
            }
        }
        stage('Remote Shell') {
            steps {
                script {
                    // Define your remote server details
                    def remoteServer = '3.94.84.42'
                    def remoteUser = 'ubuntu'
                    def remotePassword = '123456'
                    
                    // Command to execute remotely
                    def remoteCommand = 'docker push hishamkhalil/flaskapp:tagname'
                    
                    // Execute the command on the remote server
                    sh(script: """
                        sshpass -p ${remotePassword} ssh ${remoteUser}@${remoteServer} '${remoteCommand}'
                    """, returnStatus: true)
                }
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
