pipeline {
    environment{
        IMAGE_NAME = 'webapp'
        IMAGE_TAG = 'v1'
        DOCKER_PASSWORD = credentials('docker-password')
        DOCKER_USERNAME = 'ybrenala'
        HOST_PORT = 8080
        CONTAINER_PORT = 80
        IP_DOCKER = '172.17.0.1'
    }
    agent any
    stages {
        stage ('Build'){
            steps {
                script{
                    sh '''
                        docker build --no-cache -t $IMAGE_NAME:$IMAGE_TAG .
                    '''
                }
            }
        }
        stage ('Test'){
            steps {
                script{
                    sh '''
                        docker run --rm -dp $HOST_PORT:$CONTAINER_PORT --name $IMAGE_NAME $IMAGE_NAME:$IMAGE_TAG
                        sleep 5
                        curl -I http://$IP_DOCKER
                        sleep 5
                        docker stop $IMAGE_NAME
                    '''
                }
            }
        }
        stage ('Release'){
            steps {
                script{
                    sh '''
                        docker tag $IMAGE_NAME:$IMAGE_TAG $DOCKER_USERNAME/$IMAGE_NAME:$IMAGE_TAG
                        echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                        docker push $DOCKER_USERNAME/$IMAGE_NAME:$IMAGE_TAG
                    '''
                }
            }
        }
    }
}