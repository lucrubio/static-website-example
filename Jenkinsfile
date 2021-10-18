pipeline{
    environment {
        IMAGE_NAME = "static-website-example"
        IMAGE_TAG = "${BUILD_TAG}"
        CONTAINER_NAME = "Webstatic"
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
    }


}