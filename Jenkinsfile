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
                sh 'docker build -t mennahaggag/flask-docker:1.0 .'
            }
        }

        stage('Run Security Scans on Docker Image') {
            steps {
                echo 'Running security scans on the Docker image...'
                script {
                    // Run Trivy to scan the Docker image
                    def trivyOutput = sh(script: "trivy image mennahaggag/flask-docker:1.0", returnStdout: true).trim()

                    // Display Trivy scan results
                    println trivyOutput

                    // Check if vulnerabilities were found
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
                    sh "docker push mennahaggag/flask-docker:1.0"
                }
            }
        }
    }
}
