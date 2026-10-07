pipeline {
    agent any

    stages {
        stage('test') {
            steps {
                echo "Testing the application"
            }
        }
        stage('Build') {
            steps {
                echo "Building branch ${env.BRANCH_NAME}"
            }
        }
       stage('Deploy') {
    steps {
        script {
            echo 'deploying docker image to EC2...'

            def dockerComposeCmd = "docker-compose -f docker-compose.yaml up --detach"

            sshagent(credentials: ['ec2-server-key']) {

                // Copy docker-compose file to EC2
                sh "scp -o StrictHostKeyChecking=no docker-compose.yaml ec2-user@54.85.3.217:/home/ec2-user"

                // Pull latest image
                sh "ssh -o StrictHostKeyChecking=no ec2-user@54.85.3.217 'docker pull bisrat1/reactapp:${VERSION}'"

                // Run docker-compose
                sh "ssh -o StrictHostKeyChecking=no ec2-user@54.85.3.217 '${dockerComposeCmd}'"
            }
        }
    }
}

    }
}
