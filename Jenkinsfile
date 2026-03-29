pipeline {
     agent any

    environment {
        M2_HOME = '/opt/maven'
        PATH = "$M2_HOME/bin:$PATH"
        JAVA_HOME = '/usr/lib/jvm/java-21-openjdk-amd64'
    }

    stages {

        stage('Checkout') {
            steps {
git branch: 'main', 
    url: 'https://github.com/Vyakhya-1/jenkins-assignment.git', 
    credentialsId: 'github-credentials'
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
                    scp -o StrictHostKeyChecking=no target/*.jar ubuntu@44.197.239.52:/home/ubuntu/
                    
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
