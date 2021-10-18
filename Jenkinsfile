pipeline{
    environment {
        IMAGE_NAME = "static-website-example"
        IMAGE_TAG = "${BUILD_TAG}"
        CONTAINER_NAME = "Containerweb"
    }
    agent none
    stages {
        stage("build"){
            steps {
                sh """
                    docker build -t ${IMAGE_NAME} .
                """
            }
        }
        stage("run"){
            steps{
                sh """
                    docker run --name ${CONTAINER_NAME} -d -p 80:5000 -e PORT=5000 ${IMAGE_NAME}:${IMAGE_TAG}
                    sleep 5
                """
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