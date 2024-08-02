pipeline {
    agent any

    stages {
        stage('Run Security Tests') {
            steps {
                echo 'Running security tests...'
            }
        }

        stage('Create an Image') {
            steps {
                echo 'Creating an image...'
                script {
                    docker.build('mennahaggag/flask-docker:1.0')
                }
            }
        }

        stage('Run Security Scans on Docker Image') {
            steps {
                echo 'Running security scans on the Docker image...'
                sh 'trivy image mennahaggag/flask-docker:1.0'
            }
        }

        stage('Upload Docker Image to Docker Hub') {
            steps {
                echo 'Uploading Docker image to Docker Hub...'
                script {
                    docker.withRegistry('', 'dockerhub-credentials') {
                        docker.image('mennahaggag/flask-docker:1.0').push()
                    }
                }
            }
        }
    }
}
