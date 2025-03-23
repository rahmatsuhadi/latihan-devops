pipeline {
    agent any

    triggers {
        pollSCM('H/1 * * * *')  // Memeriksa perubahan setiap 1 menit
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/rahmatsuhadi/latihan-devops.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t latihan-devops-image:latest .'
            }
        }

        stage('Test') {
            steps {
                script {
                    // Cek apakah container dengan nama 'latihan-devops-container' sudah berjalan
                    def existingContainer = sh(script: "docker ps -q -f name=latihan-devops-container", returnStdout: true).trim()

                    // // Jika container sudah ada, stop dan hapus container tersebut
                    if (existingContainer) {
                        echo "Stopping and removing existing container..."
                        sh 'docker stop latihan-devops-container || true'  // Menghentikan container jika ada
                        sh 'docker rm latihan-devops-container || true'    // Menghapus container jika ada
                    }

                    // // Menjalankan container baru
                    echo "Running new container..."
                    sh 'docker run -d --name latihan-devops-container --rm -p 89:80 latihan-devops-image:latest'
                }
            }
        }
    }
}
