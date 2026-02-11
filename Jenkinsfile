pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            reuseNode true // สำคัญมาก เพื่อแชร์ Workspace ระหว่าง Stage
        }
    }

    environment {
        VERCEL_TOKEN = credentials('vercel-token') // ต้องตั้งชื่อ ID ให้ตรงกับใน Jenkins
    }

    stages {
        stage('Test npm') { // 2 คะแนน
            steps {
                sh 'npm install'
                sh 'npm test -- --watchAll=false' // ผลรันต้อง PASS แบบในรูปที่ 6
            }
        }

        stage('Build') { // 2 คะแนน
            steps {
                sh 'npm run build'
            }
        }

        stage('Test Build') { // 2 คะแนน
            steps {
                sh 'ls -al build' 
            }
        }

        stage('Deploy') { // 2 คะแนน
            steps {
                // ระบุชื่อโปรเจกต์เป็นตัวพิมพ์เล็ก และส่ง Token ผ่านตัวแปร
                sh "npx vercel --token ${VERCEL_TOKEN} --name learn-jenkins-app --prod --yes"
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!'
        }
    }
}