node {
    def app
    stage('Clone repository') {
        checkout scm
    }
    
    stage('Build image') {
       app = docker.build("swkazurecr.azurecr.io/test-web")
    }
    
    stage('Push image') {        
        docker.withRegistry('https://swkazurecr.azurecr.io', 'acr-credential') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
}
