pipeline {
    agent any

    environment {
        FIREBASE_PROJECT = 'auto-deploy01'
    }

    stages {
        stage('Clone') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                dir('auto-deploy') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Install Firebase CLI') {
            steps {
                dir('auto-deploy') {
                    sh 'npm install firebase-tools'
                }
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([string(credentialsId: 'FIREBASE_TOKEN', variable: 'FIREBASE_TOKEN')]) {
                    dir('auto-deploy') {
                        sh 'npx firebase deploy --only hosting:$FIREBASE_PROJECT --token $FIREBASE_TOKEN'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment succeeded.'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}