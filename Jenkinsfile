pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            reuseNode true // เพื่อแชร์ Workspace ตามรูปที่ 10
        }
    }

    environment {
        VERCEL_TOKEN = credentials('vercel-token') // ต้องตั้งชื่อ ID ให้ตรงกับใน Jenkins
    }

    stages {
        stage('Test npm') { // 2 คะแนน: ความสมบูรณ์ของ stage
            steps {
                sh 'npm install'
                // ใช้ --watchAll=false เพื่อให้เทสจบในรอบเดียว ไม่ค้าง
                sh 'npm test -- --watchAll=false' 
            }
        }

        stage('Build') { // 2 คะแนน: มี stages ครบถ้วน
            steps {
                sh 'npm run build'
            }
        }

        stage('Test Build') { // 2 คะแนน: ตรวจสอบผลลัพธ์การ Build
            steps {
                // เช็คว่ามีโฟลเดอร์ build เกิดขึ้นจริงไหม
                sh 'ls -al build' 
            }
        }

        stage('Deploy') { // 2 คะแนน: ผลรันเป็นสีเขียว
            steps {
                // Deploy แบบไม่ต้องใช้ --prebuilt เพื่อลดความผิดพลาด
                sh "npx vercel --token ${vcp_4zA3ZPbZ4ATZrg1CnPGvfrWZJqbBr14lDpn7dOjIel6kUKgkEr43cI3K} --prod --yes"
            }
        }
    }

    post {
        always {
            // ลบ junit ออกก่อนถ้าคุณยังไม่ได้ตั้งค่า reporter ในโปรเจกต์ เพื่อไม่ให้ Pipeline แดง
            echo 'Pipeline finished!'
        }
    }
}