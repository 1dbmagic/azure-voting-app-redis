node {
    def acrHost = 'swkazurecr.azurecr.io'          // 프로토콜 제외
    def acrUrl = "https://${acrHost}"              // 프로토콜 포함
    def acrCredential = 'acr-credential'
    def backendImage
    def frontendImage

    try {
        stage('Clone repository') {
            checkout scm
        }

        stage('Build images with Docker Compose') {
            sh 'docker-compose build'
            backendImage = "${acrHost}/azure-vote-back:${env.BUILD_NUMBER}"
            frontendImage = "${acrHost}/azure-vote-front:${env.BUILD_NUMBER}"
            sh """
            docker tag mcr.microsoft.com/oss/bitnami/redis:6.0.8 $backendImage
            docker tag mcr.microsoft.com/azuredocs/azure-vote-front:v1 $frontendImage
            """
        }

        stage('Push images to Azure Container Registry') {
            docker.withRegistry(acrUrl, acrCredential) {
                sh """
                docker push $backendImage
                docker push $frontendImage
                """
            }
        }

        stage('Clean up local images') {
            sh 'docker-compose down --rmi all'
        }
    } catch (e) {
        error "Pipeline failed: ${e}"
    } finally {
        echo 'Pipeline execution completed.'
    }
}

