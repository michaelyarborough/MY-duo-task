pipeline {

    agent any
  

    stages {

        stage('Build') {

            steps {sh '''

                docker build -t michaelyarborough/flask-duo:latest -t michaelyarborough/flask-duo:v${BUILD_NUMBER} .
                docker build -t michaelyarborough/flask-nginx:latest -t michaelyarborough/flask-nginx:v${BUILD_NUMBER} ./nginx
                '''
            }

        }
        stage('DockerPush') {

            steps {
                sh '''
                docker push michaelyarborough/flask-duo:latest 
                docker push michaelyarborough/flask-duo:v${BUILD_NUMBER}
                docker push michaelyarborough/flask-nginx:latest 
                docker push michaelyarborough/flask-nginx:v${BUILD_NUMBER}
                '''
            }

        }
        stage('Deploy') {

            steps { sh '''
                    kubectl apply -f ./kubernetes
                    kubectl rollout restart deployment/flask-deployment:v${BUILD_NUMBER}
                    kubectl rollout restart deployment/nginx-deployment:v${BUILD_NUMBER}                  
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
