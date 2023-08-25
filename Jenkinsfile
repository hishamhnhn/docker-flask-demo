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
        stage('Build free') {
            steps { 
                build 'free'
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
