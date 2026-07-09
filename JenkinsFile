pipeline{
    agent any
    tools{
        maven 'mvn3.9.9'
    }
        environment{
        DOCKER_IMAGE='simplybyte-calculator-jenkins-local'
        CONTAINER_NAME='simplybyte-container-calculator'
    }
    parameters{
            choice(name: 'ENVIRONMENT' , choices: ['testing','prod'], description: 'environemnt')
    }
    stages{
        stage('git clone'){
            steps{
                git branch: 'main', credentialsId: 'github_simplybyte_credentials', url: 'https://github.com/simplybyte1266/simplybyte-springboot.git'
            }
        }
        stage('print path'){
            steps{
                sh 'echo $PATH'
            }
        }
        stage('maven test stage'){
            when{
                expression{
                    params.ENVIRONMENT == 'testing'
                }
            }
            steps{
                sh 'mvn test'
            }
        }
        stage('maven build stage'){
            steps{
                sh 'mvn clean install -DskipTests=true'
            }
        }
        stage('Docker image build'){
            steps{
                sh 'docker build -t $DOCKER_IMAGE:$BUILD_NUMBER .'
            }
        }
        stage('Docker old container remove'){
            steps{
                sh 'docker rm -f $CONTAINER_NAME || true'
            }
        }
        stage('Docker new container run'){
            steps{
                sh 'docker run -d -p 8091:8090 --name=$CONTAINER_NAME $DOCKER_IMAGE:$BUILD_NUMBER'
            }
        }
    }
}
