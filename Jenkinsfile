
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
             post {
                always {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                    }
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
        stage('analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                        sh 'mvn sonar:sonar'
                }
            }
        }
    }

}
