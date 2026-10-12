pipeline {
    agent any

    environment {
        registry = "haleema143/tp2"
        registryCredential = 'docker_hub'
    }

    stages {
        stage('Cloning Git') {
            steps {
                git 'https://github.com/HaleemaOKSM/TP2_JENKINS.git'
            }
        }

        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Test image') {
            steps {
                script {
                    bat "docker image inspect ${registry}:${BUILD_NUMBER}"
                    echo "Tests passed"
                }
            }
        }

        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy image') {
            steps {
                bat '''
                    set DOCKER_HOST=
                    docker rm -f tp2-web 2>NUL
                    docker run -d --name tp2-web -p 8081:80 %registry%:%BUILD_NUMBER%
                    if errorlevel 1 exit /b 1
                '''
            }
        }
    }
}