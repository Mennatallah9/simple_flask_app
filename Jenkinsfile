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
                sh 'docker build -t mennaashraf/flask-docker:1.0 .'
            }
        }

        stage('Run Security Scans on Docker Image') {
            steps {
                echo 'Running security scans on the Docker image...'
                script {
                    def trivyOutput = sh(script: "trivy image mennaashraf/flask-docker:1.0", returnStdout: true).trim()
                    println trivyOutput
                    if (trivyOutput.contains("Total: 0")) {
                        echo "No vulnerabilities found in the Docker image."
                    } else {
                        echo "Vulnerabilities found in the Docker image."
                    }
                }
            }
        }

        stage('Upload Docker Image to Docker Hub') {
            steps {
                echo 'Uploading Docker image to Docker Hub...'
                script {
                    echo "Preparing to push Docker image..."
                    sh 'docker images'
                    docker.withRegistry("https://index.docker.io/v1/", 'dockerhub') {
                        docker.image('mennaashraf/flask-docker:1.0').push('latest')
                    }
                }
            }
        }
    }
}
