# Demo
JenkinIntegrationTest
pipeline {

    agent any

    stages {

        stage('Deploy BLUE') {

            steps {

                echo "=== BLUE-GREEN: Deploy BLUE ==="

                bat '''

                echo Hello from BLUE v1 > current-env.txt

                type current-env.txt

                '''

            }

        }
 
        stage('Deploy GREEN') {

            steps {

                input message: "Ready to deploy GREEN environment?"

                bat '''

                echo Hello from GREEN v2 > green-env.txt

                type green-env.txt

                '''

            }

        }
 
        stage('Switch Traffic') {

            steps {

                input message: "Switch traffic from BLUE -> GREEN?"

                bat '''

                copy green-env.txt current-env.txt >nul

                echo Current traffic now points to:

                type current-env.txt

                '''

            }

        }
 
        stage('Cleanup BLUE') {

            steps {

                input message: "Cleanup BLUE environment?"

                bat 'del green-env.txt >nul'

            }

        }

    }
 
