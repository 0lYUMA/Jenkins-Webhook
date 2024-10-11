pipeline {
    agent any

    environment {
        RDS_URL = credentials('RDS_URL')
        RDS_USER = credentials('RDS_USER')
        RDS_PWD = credentials('RDS_PWD')
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Gradle Wrapper에 실행 권한 부여
                    sh 'chmod +x ./gradlew' 

                    // Gradle 빌드 실행
                    sh "./gradlew build -Drds.url=${env.RDS_URL} -Drds.username=${env.RDS_USER} -Drds.password=${env.RDS_PWD}"
                }
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
