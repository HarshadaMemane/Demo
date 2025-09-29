# Demo
JenkinIntegrationTest
node {
    def imageName = "my-dotnet"
    def tag = "v${env.BUILD_NUMBER}"
    def fullImage = "${imageName}:${tag}"
    def testPort = "8086"

    stage('Build Image') {
        bat "docker build -t ${fullImage} C:\\DockerDemo"
    }

    stage('Run Container') {
        bat "docker rm -f dotnetdemo || ver>nul"
        bat "docker run -d --name dotnetdemo -p ${testPort}:80 ${fullImage}"
        bat 'powershell -NoLogo -NoProfile -Command "Start-Sleep -Seconds 5"'
    }

    stage('Smoke Test') {
        bat """powershell -NoLogo -NoProfile -Command "Invoke-WebRequest http://localhost:${testPort}/ -UseBasicParsing | Select-Object -ExpandProperty StatusCode" """
    }

    stage('Manual Approval to Cleanup') {
        input message: "App is running at http://localhost:${testPort}. Test manually and click Proceed to cleanup.", ok: "Proceed"
    }

    stage('Cleanup') {
        bat "docker rm -f dotnetdemo || ver>nul"
    }
}
