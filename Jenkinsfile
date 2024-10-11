pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    // Gradle Wrapper에 실행 권한 부여
                    sh 'chmod +x ./gradlew'
                    // Spring Boot 빌드
                    sh './gradlew build'
                }
            }
        }

        stage('Upload to S3') {
            steps {
                script {
                    // 빌드된 Jar 파일을 S3로 업로드
                    sh 'aws s3 cp build/libs/step18_empApp-0.0.1-SNAPSHOT.jar https://ce25-bucket-01.s3.ap-northeast-2.amazonaws.com/'
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    // EC2 인스턴스에 SSH로 접속하여 Jar 파일 다운로드 및 실행
                    sshagent (credentials: ['ec2-ssh-key']) {
                        sh """
                        ssh -o StrictHostKeyChecking=no ubuntu@13.125.231.38 << 'EOF'
                        aws s3 cp https://ce25-bucket-01.s3.ap-northeast-2.amazonaws.com/step18_empApp-0.0.1-SNAPSHOT.jar /home/ubuntu/
                        nohup java -jar /home/ubuntu/step18_empApp-0.0.1-SNAPSHOT.jar > /dev/null 2>&1 &
                        EOF
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build or deployment failed.'
        }
    }
}
