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
                  def dockerCmd = 'docker run -p 3080:3080 bisrat1/reactapp:1.0'
                  sshagent(credentials: ['ec2-server-key'], executable: '') {
                    // some block
                     sh "ssh -o StrictHostKeyChecking=no ec2-user@54.85.3.217 ${dockerCmd}"
                     }
                   }
              }
             
        }
    }
}
