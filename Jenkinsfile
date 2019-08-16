
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -Dversion=${BUILD_NUMBER} -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test -Dversion=${BUILD_NUMBER}'
                archiveArtifacts 'target/surefire-reports/*.xml'
            }
            post {
                always {
                    tapdTestReport frameType: 'JUnit', onlyNewModified: true, reportPath: 'target/surefire-reports/*.xml'
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
        
        stage('Package'){
            steps{
                nexusPublisher nexusInstanceId: '199', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/*.jar']], mavenCoordinate: [artifactId: 'my-app', groupId: 'william', packaging: 'jar', version: '${BUILD_NUMBER}']]]
            }
        }
        stage('Deliver') {
            steps {
                sh 'sh ./deliver.sh'
            }
        }
    }
}
