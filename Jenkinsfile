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
                    withCredentials([string(credentialsId: 'RDS_URL', variable: 'DB_URL'),
                                     string(credentialsId: 'RDS_USER', variable: 'DB_USER'),
                                     string(credentialsId: 'RDS_PWD', variable: 'DB_PWD')]) {
                        sh "./gradlew build -Drds.url=\$DB_URL -Drds.username=\$DB_USER -Drds.password=\$DB_PWD"
                    }
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
