pipeline {
    agent { label 'maven-agent' }

    environment {
        M2_HOME = '/opt/maven'
        JAVA_HOME = '/usr/lib/jvm/java-21-openjdk-amd64'
        PATH = "/opt/maven/bin:/usr/lib/jvm/java-21-openjdk-amd64/bin:${env.PATH}"
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
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['app-server-key']) {
                    sh '''
                    # Define JAR name explicitly
                    JAR_FILE=target/jenkins-assignment-1.0-SNAPSHOT.jar

                    # Copy the JAR to the app server
                    scp -o StrictHostKeyChecking=no $JAR_FILE ubuntu@44.197.239.52:/home/ubuntu/app.jar

                    # Run the JAR in background and redirect logs
                    ssh -o StrictHostKeyChecking=no ubuntu@44.197.239.52 "nohup java -jar /home/ubuntu/app.jar > /home/ubuntu/app.log 2>&1 &"
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
