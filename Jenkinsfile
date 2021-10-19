pipeline{
    environment {
        IMAGE_NAME = "static-website-example"
        IMAGE_TAG = "${BUILD_TAG}"
        CONTAINER_NAME = "Webstatic"
        USERNAME = "Luc Rubio"
    }
    agent none
    stages {
        stage ('Build Image'){
            agent any
            steps{
                script{
                    sh 'docker build -t ${IMAGE_NAME} .'
                }
            }
        }
        stage ('Run container based on Builded image'){
            agent any
            steps{
                script{
                    sh '''
                        docker run --name ${CONTAINER_NAME} -d -p 8082:5555 -e PORT=5555 ${IMAGE_NAME}:${IMAGE_TAG}
                        sleep 5
                    ''' 
                }
            }
        }
        stage ('Test Image'){
            agent any
            steps{
                script{
                    sh '''
                       curl http://172.17.0.1 | grep -q "Welcome"
                    '''
                }
            }
        }

        stage('Clean Container') {
        agent any
            steps {
                script {
                    sh '''
                       docker stop ${CONTAINER_NAME}
                       docker rm ${CONTAINER_NAME}
                    '''
                }
            }
        }
    
    stage('Push image to Dockerhub') {
            agent any
            steps {
                script {
                    sh '''
                       docker login -u ${USERNAME} -p ${PASSWORD}
                       docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    '''
                }
            }
        }
    }

    
}