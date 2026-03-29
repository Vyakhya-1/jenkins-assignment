pipeline {
    agent { label 'maven-agent' }

    stages {

        stage('Checkout') {
            steps {sshagent(['github-credentials']) {
            sh 'ssh-keyscan github.com >> ~/.ssh/known_hosts'
        }
                // Pull code from GitHub using SSH
                git branch: 'main', url: 'git@github.com:Vyakhya-1/jenkins-assignment.git'
            }
        }

        stage('Build') {
            steps {
                // Build Java Maven project
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                // Use Jenkins SSH credential for App Server
                sshagent(['app-server-key']) {
                    sh '''
                    # Copy the JAR to the app server
                    scp -o StrictHostKeyChecking=no target/*.jar ubuntu@<APP-IP>:/home/ubuntu/
                    
                    # Run the JAR in background
                    ssh -o StrictHostKeyChecking=no ubuntu@44.197.239.52 "java -jar /home/ubuntu/*.jar &"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline finished successfully!'
        }
        failure {
            echo 'Pipeline Failed!'
        }
    }
}
