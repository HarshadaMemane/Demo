# Demo
JenkinIntegrationTest
pipeline {
    agent any
    stages {
        stage('Deploy Stable') {
            steps {
                echo "=== CANARY: Stable v1 ==="
                bat '''
                echo Hello from STABLE v1 > stable.txt
                type stable.txt
                '''
            }
        }
 
        stage('Deploy Canary') {
            steps {
                input message: "Deploy CANARY v2 (10% traffic)?"
                bat '''
echo Hello from CANARY v2 > canary.txt
                echo Simulating traffic split (4 stable, 1 canary):
                type stable.txt
                type stable.txt
                type stable.txt
                type stable.txt
                type canary.txt
                '''
            }
        }
 
        stage('Increase Canary') {
            steps {
                input message: "Increase CANARY to 50%?"
                bat '''
                echo Simulating 50-50 traffic:
                type stable.txt
                type canary.txt
                type stable.txt
                type canary.txt
                '''
            }
        }
 
        stage('Promote Canary') {
            steps {
                input message: "Promote CANARY to 100% (make stable)?"
                bat '''
                copy canary.txt stable.txt >nul
                type stable.txt
                del canary.txt >nul
                '''
            }
        }
    }
    post {
        always {
            bat 'del stable.txt 2>nul'
            echo "Demo cleaned up!"
        }
    }
}
