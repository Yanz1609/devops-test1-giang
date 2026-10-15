
pipeline {
    agent any

    environment {
        PROJECT_NAME = 'devops-test1-giang'
        WEBSITE_URL = 'https://devops-test1-giang.vercel.app'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install') {
            steps {
                echo 'Static HTML - no dependencies required'
            }
        }

        stage('Build') {
            steps {
                sh 'test -f index.html'
                echo 'BUILD SUCCESS'
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([
                    string(credentialsId: 'vercel-deploy-hook', variable: 'DEPLOY_HOOK'),
                    string(credentialsId: 'telegram-bot-token', variable: 'BOT_TOKEN'),
                    string(credentialsId: 'telegram-chat-id', variable: 'CHAT_ID')
                ]) {
                    sh '''
                        curl -sS -X POST \
                          --data-urlencode "chat_id=$CHAT_ID" \
                          --data-urlencode "text=🚀 DEPLOY STARTED
Project: devops-test1-giang
Branch: main" \
                          "https://api.telegram.org/bot$BOT_TOKEN/sendMessage"

                        curl --fail --silent --show-error -X POST "$DEPLOY_HOOK"
                    '''
                }
            }
        }
    }

    post {
        success {
            withCredentials([
                string(credentialsId: 'telegram-bot-token', variable: 'BOT_TOKEN'),
                string(credentialsId: 'telegram-chat-id', variable: 'CHAT_ID')
            ]) {
                sh '''
                    curl -sS -X POST \
                      --data-urlencode "chat_id=$CHAT_ID" \
                      --data-urlencode "text=✅ DEPLOY SUCCESS NHÉ
Project: devops-test1-giang
Branch: main
URL: https://devops-test1-giang.vercel.app" \
                      "https://api.telegram.org/bot$BOT_TOKEN/sendMessage"
                '''
            }
        }

        failure {
            withCredentials([
                string(credentialsId: 'telegram-bot-token', variable: 'BOT_TOKEN'),
                string(credentialsId: 'telegram-chat-id', variable: 'CHAT_ID')
            ]) {
                sh '''
                    curl -sS -X POST \
                      --data-urlencode "chat_id=$CHAT_ID" \
                      --data-urlencode "text=❌ DEPLOY FAILED RỒI
Project: devops-test1-giang
Branch: main
Please check Jenkins." \
                      "https://api.telegram.org/bot$BOT_TOKEN/sendMessage"
                '''
            }
        }
    }
}
