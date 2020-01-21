pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
		stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
		stage('Maven Sonarqube analysis') {
            steps {
                withSonarQubeEnv('sonar-TOCO') {
                    sh "mvn sonar:sonar -B"
                 }
            }
        }
        stage('Check Sonarqube quality gate ') {
            agent none // No need to claim an executor while we wait for Sonarqube to complete the analysis.
            steps {
                timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
                    script {
                        def qg = waitForQualityGate() // Waits for analysis result from Sonarqube. Reuses taskId previously created in the Sonarqube analysis stage
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        }
		stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}