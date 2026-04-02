pipeline {
    agent any

    options {
        timeout(time: 30, unit: 'MINUTES') // set timeout cho toàn pipeline
        timestamps() // thêm timestamp vào log
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

    }

    stage('Install and Lint and Build') {
        agent {
            docker {
                image 'node:22-alpine'
                reuseNode true // reuse container để giữ cache node_modules
            }
        }

        steps {
            sh 'node -v && npm -v'
            sh 'npm ci'
            sh 'npm run lint'
            sh 'npm run build'
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'dist/**/*', fingerprint: true, allowEmptyArchive: false
        }
        failure {
            echo 'Build failed! Read each stage logs for details.'
        }
    }
}