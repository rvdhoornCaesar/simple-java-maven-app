pipeline {
    agent {
        docker {
            image 'maven:3-jdk-11'
            reuseNode true 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
    }
}