pipeline {
    agent any

    environment {
        // Replace with your Docker Hub username
        DOCKER_HUB_REPO = 'your_dockerhub_username'
        ROLL_NUMBER = '2472'
    }

    stages {
        stage('Checkout Code 2472') {
            steps {
                git credentialsId: 'github-credentials-id', url: 'https://github.com/your_username/your_repository.git'
            }
        }

        stage('Install Dependencies 2472') {
            steps {
                sh 'cd client && yarn install'
                sh 'cd Auth && yarn install'
                sh 'cd Classrooms && yarn install'
            }
        }

        stage('Build Docker Images 2472') {
            steps {
                script {
                    def imageNames = ['frontend', 'backend', 'classrooms']
                    imageNames.each { name ->
                        sh """
                            docker build -t $DOCKER_HUB_REPO/${name}:latest -f ${name}/Dockerfile .
                        """
                    }
                }
            }
        }

        stage('Push Docker Images 2472') {
            steps {
                script {
                    def imageNames = ['frontend', 'backend', 'classrooms']
                    withCredentials([string(credentialsId: 'docker-hub-credentials-id', variable: 'DOCKER_HUB_PASSWORD')]) {
                        sh 'echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_REPO --password-stdin'
                        imageNames.each { name ->
                            sh """
                                docker push $DOCKER_HUB_REPO/${name}:latest
                            """
                        }
                    }
                }
            }
        }

        stage('Deploy and Validate 2472') {
            steps {
                sh 'docker-compose down'
                sh 'docker-compose up -d'
                // Add additional steps to validate your application
            }
        }
    }

    post {
        always {
            sh 'docker-compose down'
        }
    }
}
