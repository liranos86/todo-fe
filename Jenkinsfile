pipeline { 
    agent {label 'docker'}
    environment{
        DOCKERHUB_CREDENTIALS=credentials('dockerhub')
    }
    stages{
        stage('Build stage'){
            steps{
                deleteDir()
                sh 'DOCKER_BUILDKIT=1 docker build -f Dockerfile-pipelines -t image-builder --target builder .'
            }
        }
        stage ('Test Stage'){
            steps{
                 sh 'DOCKER_BUILDKIT=1 docker build -f Dockerfile-pipelines -t image-testing --target testing .'
            }
        }
        stage ('Delivery Stage'){
	    steps{
                 sh 'DOCKER_BUILDKIT=1 docker build -f Dockerfile-pipelines -t liranos86/todo-fe:jenkins-$BUILD_NUMBER --target delivery .'
	    }
        }
        stage('Cleanup') {
            steps {
                echo "Clean It All"
                sh 'docker system prune -f'
            }
        }

        stage ('login') {
            steps{
                sh 'docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW'
            }
        }
        stage('push') {
            steps{
                sh "docker push liranos86/todo-fe:jenkins-${BUILD_NUMBER}"
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
    }
}
