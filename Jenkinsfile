pipeline {
    agent none
    stages {
        stage('Integration UI Test') {
            parallel {
                stage('Deploy') {
                    agent any
                    steps {
                        sh 'chmod +x ./jenkins/scripts/deploy.sh'
                        sh './jenkins/scripts/deploy.sh'
                        // Automatically proceed without manual input
                        input message: 'Proceed?', submitter: 'auto'
                    }
                }
                stage('Headless Browser Test') {
                    agent {
                        docker {
                            image 'maven:3-alpine'
                            args '-v /root/.m2:/root/.m2'
                        }
                    }
                    steps {
                        sh 'mvn -B -DskipTests clean package'
                        sh 'mvn test'
                    }
                    post {
                        always {
                            junit 'target/surefire-reports/*.xml'
                        }
                    }
                }
            }
        }
    }
}