pipeline {
    agent any

    parameters {
        string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Docker image tag')
        booleanParam(name: 'PUSH_IMAGE', defaultValue: true, description: 'Push image to Docker Hub?')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'staging', 'prod'], description: 'Select the environment')
    }
    
    environment {
        IMAGE_NAME = "devopssteps/my-app-15"
    }

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning code from GitHub............'
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                echo 'Hello World 111 - Building application'
                echo "Environment: ${params.ENVIRONMENT}"
                echo "Image Name: ${IMAGE_NAME}"
                echo "Image Tag: ${params.IMAGE_TAG}"
            }
        }
        
        stage('Test') {
            steps {
                echo 'Hello World test222 - Running tests'
                echo 'All tests passed!'
            }    
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image: ${IMAGE_NAME}:${params.IMAGE_TAG}"
                    // Uncomment jodi Docker build korte chan:
                    // sh "docker build -t ${IMAGE_NAME}:${params.IMAGE_TAG} ."
                }
            }
        }
        
        stage('Push Docker Image') {
            when {
                expression { return params.PUSH_IMAGE }
            }
            steps {
                echo "Pushing Docker image: ${IMAGE_NAME}:${params.IMAGE_TAG}"
                // Uncomment jodi push korte chan:
                // sh "docker push ${IMAGE_NAME}:${params.IMAGE_TAG}"
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    if (params.ENVIRONMENT == 'dev') {
                        echo "🚀 Deploying to Development environment"
                        echo "Dev deployment logic here..."
                    } else if (params.ENVIRONMENT == 'staging') {
                        echo "🚀 Deploying to Staging environment"
                        echo "Staging deployment logic here..."
                    } else if (params.ENVIRONMENT == 'prod') {
                        echo "🚀 Deploying to Production environment"
                        echo "Production deployment logic here..."
                    } else {
                        error("Invalid environment selected!")
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline succeeded!'
            echo 'All stages completed successfully.'
        }
        failure {
            echo '❌ Pipeline failed.'
            echo 'Please check the logs above for errors.'
        }
        always {
            echo '🏁 Pipeline execution completed.'
            echo "Total stages executed: Clone, Build, Test, Build Image, Push Image, Deploy"
        }
    }
}
