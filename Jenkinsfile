pipeline {
    environment {
        IMAGE_NAME = "alpinehelloworld"
        IMAGE_TAG = "v2"
        PREFIX_IMAGE = "abodojustin"
        STAGING = "jenkins-francis-staging"
        PRODUCTION = "jenkins-francis-production"
    }
    agent none
    stages {
        stage('Build Image') {
            agent any
            steps {
                script {
                    sh 'docker build -t $PREFIX_IMAGE/$IMAGE_NAME:$IMAGE_TAG .'
                }
            }
        }
        stage('Run Container based on Build Image') {
            agent any
            steps {
                script {
                    sh '''
                        docker run --name $IMAGE_NAME -d -p 80:5000 -e PORT=5000 $PREFIX_IMAGE/$IMAGE_NAME:$IMAGE_TAG
                        sleep 10
                    '''
                }
            }
        }
        stage('Test Image') {
            agent any
            steps {
                script {
                    sh '''
                        curl http://3.95.39.174 | grep -q "Hello world!"
                    '''
                }
            }
        }
        stage('Clean Container') {
            agent any
            steps {
                script {
                    sh '''
                        docker rm -f ${IMAGE_NAME}
                    '''
                }
            }
        }
        /* stage('Push Image in Staging And Deploy It') {
            when {
                expression { GIT_BRANCH == 'origin/master' }
            }
            agent any
            environment {
                HEROKU_API_KEY = credentials('HEROKU_API_KEY')
            }
            steps {
                script {
                    sh '''
                        heroku container:login
                        heroku create $STAGING || echo "projetc already exist"
                        heroku container:push -a $STAGING web
                        heroku container:release -a $STAGING web
                    '''
                }
            }
        }
        stage('Push Image in Production And Deploy It') {
            when {
                expression { GIT_BRANCH == 'origin/master' }
            }
            agent any
            environment {
                HEROKU_API_KEY = credentials('HEROKU_API_KEY')
            }
            steps {
                script {
                    sh '''
                        heroku container:login
                        heroku create $PRODUCTION || echo "projetc already exist"
                        heroku container:push -a $PRODUCTION web
                        heroku container:release -a $PRODUCTION web
                    '''
                }
            }
        } */
    }
}