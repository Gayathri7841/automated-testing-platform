pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {    git branch: 'main', url: 'https://github.com/Gayathri7841/automated-testing-platform.git' }
        }
        stage('Test') {
            steps {
                script {
                    if (fileExists('pom.xml')) {
                        sh 'mvn clean test'
                    } else if (fileExists('package.json')) {
                        sh 'npm install'
                        sh 'npm test'
                    }
                }
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        stage('Send Report') {
            steps {
                sh '''
                curl -X POST -H "Content-Type: application/json" \
                    -d '{"buildId": "${BUILD_ID}", "commitId": "${GIT_COMMIT}"}' \
                    http://localhost:8080/api/builds
                '''
            }
        }
    }
}
