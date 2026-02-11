pipeline {

    agent {
        docker {
            image 'node:18-alpine'
            reuseNode true
        }
    }

    environment {
        VERCEL_PROJECT_NAME = 'learn-jenkins-app-lac' // เปลี่ยนเป็น Project Name ของคุณ
        VERCEL_TOKEN = credentials('vercel-token') // ดึงจาก Jenkins Credentials ที่ชื่อ vercel-token
    }

    stages {

        stage('Build') {
            steps {
                sh 'npm ci'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                // ติดตั้ง Vercel CLI 
                sh 'npm install vercel'

                // สั่ง Deploy ไปยัง Vercel 
                // ใช้ --prebuilt เพื่อระบุว่าไฟล์ในโฟลเดอร์ build/ ได้ถูกสร้างไว้แล้ว
                sh './node_modules/.bin/vercel deploy --prod --prebuilt'
            }
        }

    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
