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
                    def remoteServer = '18.232.79.173'
                    def remoteUser = 'ubuntu'
                    def remotePassword = '123456'
                    // Directory path to create on the remote server
                    def remoteDirectory = '/home/ubuntu'
            
                    // Command to create the directory remotely
                    def remoteCommand = "mkdir -p ${remoteDirectory}"
                    // Command to execute remotely
                    def remoteCommand = 'mkdir test'
                    
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

