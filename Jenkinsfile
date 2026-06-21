pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t myapp:${BUILD_NUMBER} .'
            }
        }

        stage('Test') {
            steps {
                sh 'docker run --rm myapp:${BUILD_NUMBER} echo "Tests passed"'
            }
        }

        stage('Approval') {
            when {
                branch 'develop'
            }
            steps {
                input message: 'Tests passed. Approve to merge to master?', ok: 'Approve'
            }
        }

        stage('Deploy to Prod') {
            when {
                branch 'master'
            }
            steps {
                sh 'docker stop prod || true'
                sh 'docker rm prod || true'
                sh 'docker run -d --name prod -p 82:80 myapp:${BUILD_NUMBER}'
            }
        }
    }
}
