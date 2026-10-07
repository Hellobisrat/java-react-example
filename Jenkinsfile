pipeline {
    agent any

    tools {
        // remove maven, your project is gradle
        // maven 'maven-3.9'
    }

    environment {
        VERSION = "1.0"
    }

    stages {

        stage('test') {
            steps {
                echo "Testing the application"
            }
        }

        stage('Build jar') {
            steps {
                echo "Building the application version ${VERSION}"
                sh "./gradlew clean build"
            }
        }

        stage('Docker Build & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId:'docker-hub-repo',
                    passwordVariable:'PASSWORD',
                    usernameVariable:'USERNAME'
                )]) {
                    sh """
                        docker build -t bisrat1/reactapp:${VERSION} .
                        echo \$PASSWORD | docker login -u \$USERNAME --password-stdin
                        docker push bisrat1/reactapp:${VERSION}
                    """
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'deploying docker image to EC2...'

                    def dockerComposeCmd = "docker-compose -f docker-compose.yaml up --detach"

                    sshagent(credentials: ['ec2-server-key']) {

                        sh "scp -o StrictHostKeyChecking=no docker-compose.yaml ec2-user@54.85.3.217:/home/ec2-user"

                        sh "ssh -o StrictHostKeyChecking=no ec2-user@54.85.3.217 'docker pull bisrat1/reactapp:${VERSION}'"

                        sh "ssh -o StrictHostKeyChecking=no ec2-user@54.85.3.217 '${dockerComposeCmd}'"
                    }
                }
            }
        }
    }
}
