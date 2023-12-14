pipeline {

    agent any
  

    stages {

        stage('Build') {

            steps {sh '''

                docker build -t michaelyarborough/flask-duo:latest -t michaelyarborough/flask-duo:v${BUILD_NUMBER} .
                '''
            }

        }
        stage('DockerPush') {

            steps {
                sh '''
                docker push michaelyarborough/flask-duo:latest 
                docker push michaelyarborough/flask-duo:v${BUILD_NUMBER}
                '''
            }

        }
        stage('Deploy') {

            steps { sh '''
                    kubectl apply -f ./kubernetes
                    kubectl rollout restart deployment/flask-deployment
                    kubectl rollout restart deployment/nginx-deployment                   
                '''
            }

        }
        stage('CleanUp') {

            steps {

                sh 'docker system prune -f'
                
            }

        }
      
    }

}
