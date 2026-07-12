pipeline {
    agent any

    tools {
        maven 'maven3'
    }

    stages {

        stage('Java Version') {
            steps {
                sh 'java -version'
                sh 'javac -version'
                sh 'mvn -version'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sq1') {
                    sh '''
                        mvn sonar:sonar \
                        -Dsonar.projectKey=time-tracker \
                        -Dsonar.projectName="Time Tracker"
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
