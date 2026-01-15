// ==========================================
// JENKINS PIPELINE EXAMPLES - FULL GUIDE
// ==========================================

// 1. SIMPLE DECLARATIVE PIPELINE
pipeline {
    agent any

    stages {
        stage('build') {
            steps {
                echo 'Hello World 111'
            }
        }
        stage('test') {
            steps {
                echo 'Hello World test222'
            }    
        }
    }
}

// ==========================================

// 2. PIPELINE WITH ENVIRONMENT VARIABLES
pipeline {
    agent any
    environment {
        IMAGE_NAME = "devopssteps/my-app"
    }
    stages {
        stage('build') {
            steps {
                echo "${IMAGE_NAME}"
            }
        }
        stage('test') {
            steps {
                echo 'Hello World test222'
            }    
        }
    }
}

// ==========================================

// 3. PIPELINE WITH PARAMETERS & IF-ELSE CONDITIONS
pipeline {
    agent any

    parameters {
        choice(name: 'ENVIRONMENT', choices: ['dev', 'staging', 'prod'], description: 'Select the environment to deploy')
    }

    stages {
        stage('Print Selected Environment') {
            steps {
                echo "Selected Environment: ${params.ENVIRONMENT}"
            }
        }

        stage('Conditional Execution') {
            steps {
                script {
                    if (params.ENVIRONMENT == 'dev') {
                        echo "Deploying to Development environment"
                        // Add dev deployment logic here
                    } else if (params.ENVIRONMENT == 'staging') {
                        echo "Deploying to Staging environment"
                        // Add staging deployment logic here
                    } else if (params.ENVIRONMENT == 'prod') {
                        echo "Deploying to Production environment"
                        // Add production deployment logic here
                    } else {
                        error("Invalid environment selected!")
                    }
                }
            }
        }
    }
}

// ==========================================

// 4. PIPELINE WITH WHEN CONDITION & DOCKER BUILD
pipeline {
    agent any

    parameters {
        string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Docker image tag')
        booleanParam(name: 'PUSH_IMAGE', defaultValue: true, description: 'Push image to Docker Hub?')
    }
    environment {
        IMAGE_NAME = "devopssteps/my-app-15"
    }

    stages {
        stage('clone') {
            steps {
                echo 'clone code............'
                checkout scm
            }
        }
        stage('build image') {
            steps {
                echo "Building Docker image with tag: ${params.IMAGE_TAG}"
                sh "docker build -t ${IMAGE_NAME}:${params.IMAGE_TAG} ."
            }
        }
        stage('push image') {
            when {
                expression { return params.PUSH_IMAGE }
            }
            steps {
                echo "Pushing Docker image with tag: ${params.IMAGE_TAG}"
            }
        }
    }
}

// ==========================================

// 5. COMPLETE PIPELINE WITH POST BLOCK
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
        stage('clone') {
            steps {
                echo 'Cloning code............'
                checkout scm
            }
        }
        
        stage('build image') {
            steps {
                echo "Building Docker image with tag: ${params.IMAGE_TAG}"
                sh "docker build -t ${IMAGE_NAME}:${params.IMAGE_TAG} ."
            }
        }
        
        stage('push image') {
            when {
                expression { return params.PUSH_IMAGE }
            }
            steps {
                echo "Pushing Docker image with tag: ${params.IMAGE_TAG}"
                // sh "docker push ${IMAGE_NAME}:${params.IMAGE_TAG}"
            }
        }
        
        stage('deploy') {
            steps {
                script {
                    if (params.ENVIRONMENT == 'dev') {
                        echo "Deploying to Development environment"
                    } else if (params.ENVIRONMENT == 'staging') {
                        echo "Deploying to Staging environment"
                    } else if (params.ENVIRONMENT == 'prod') {
                        echo "Deploying to Production environment"
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded! ✅'
        }
        failure {
            echo 'Pipeline failed. ❌'
        }
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
