pipeline {
    agent none
    stages {
        stage('Maven Build') {
            agent { 
                docker {
                    image 'maven:3-jdk-11'
                    reuseNode true 
                }
            }
            steps {
                sh "mvn clean install -B"
            }
        }
	}
}