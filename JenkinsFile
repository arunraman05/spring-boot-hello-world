pipeline {
    agent any

    // This tells Jenkins to inject your configured Maven tool path automatically
    tools {
        maven 'M3' // Must match the exact name you specified in Jenkins Tools
    }

    environment {
        DOCKER_IMAGE = 'spring-boot-hello-world:1.0'
        // Keeping this variable here in case your Windows environment requires localhost routing
        DOCKER_HOST = 'tcp://localhost:2375' 
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/arunraman05/spring-boot-hello-world.git', branch: 'main'
            }
        }

        stage('Maven Build') {
            steps {
                bat 'mvn clean install -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    bat "docker build -t ${env.DOCKER_IMAGE} ."
                }
            }
        }

        stage('Docker Run') {
            steps {
                script {
                    // Added -d flag so the container runs in detached mode background. 
                    // Otherwise, Jenkins will hang indefinitely keeping the pipeline running.
                    bat "docker run -d -p 8080:8080 ${env.DOCKER_IMAGE}"
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed and application is running on port 8080!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
