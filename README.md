# Demo
JenkinIntegrationTest
pipeline {
    agent any
    stages {
        stage('Start v1 Servers') {
            steps {
                echo "=== ROLLING UPDATE: v1 ==="
                bat '''
                echo Hello from SERVER1 v1 > s1.txt
                echo Hello from SERVER2 v1 > s2.txt
                echo Hello from SERVER3 v1 > s3.txt
                type s1.txt & type s2.txt & type s3.txt
                '''
            }
        }
 
        stage('Update Server 1') {
            steps {
                input message: "Update SERVER1 to v2?"
                bat 'echo Hello from SERVER1 v2 > s1.txt & type s1.txt'
}
        }
 
        stage('Update Server 2') {
            steps {
                input message: "Update SERVER2 to v2?"
                bat 'echo Hello from SERVER2 v2 > s2.txt & type s2.txt'
            }
        }
 
        stage('Update Server 3') {
            steps {
                input message: "Update SERVER3 to v2?"
                bat 'echo Hello from SERVER3 v2 > s3.txt & type s3.txt'
            }
        }
    }
    post {
        always {
            bat 'del s1.txt s2.txt s3.txt 2>nul'
            echo "Demo cleaned up!"
        }
    }
}

