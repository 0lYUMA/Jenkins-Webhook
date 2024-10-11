pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/0lYUMA/Jenkins-Webhook.git'
            }
        }
        
        stage('Build') {
            steps {
                script {
                    // 빌드 단계에서 환경 변수를 이용하여 빌드 수행
                    sh "./gradlew build -Drds.url=${env.RDS_URL} -Drds.username=${env.RDS_USER} -Drds.password=${env.RDS_PWD}"
                }
            }
        }
    }
}
