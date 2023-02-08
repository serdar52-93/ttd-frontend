pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh '''
                  docker build -t serdar52/java-17-demo:jenkins-${BUILD_NUMBER} .
                '''
            }
        }
        
        stage('Release') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhubcredentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
  
                sh ''' 
                docker login -u $USERNAME -p $PASSWORD
                docker push serdar52/java-17-demo:jenkins-${BUILD_NUMBER}
                '''
                }
            }
        }

        stage('deploy') {
            steps {
                sh ''' 
                docker stop ttd-frontend || true
                docker rm -f ttd-frontend || true
                docker run -p3000:80 -d -- name ttd-frontend serdar52/java-17-demo:jenkins-${BUILD_NUMBER}
                '''
                }
            }
        
    }
}