code {
    def app
    # git repository를 '/var/lib/jenkins/workspace/{job name}' 위치로 clone
    stage('Clone repository') {
        checkout scm
    }
    
    # dockerfile을 이용해 'myacr.azurecr.io/test-web'라는 이름으로 이미지 빌드 ({ACR 이름}/{이미지 이름})
    # Jenkinsfile에서 push 할 때 이미지 태그를 설정하므로 여기선 이미지 이름만 설정
    stage('Build image') {
       app = docker.build("swkazurecr.azurecr.io/test-web")
    }
    
    # ACR 주소와 앞선 단계에서 Jenkins에서 생성한 ACR credential 지정
    stage('Push image') {        
        docker.withRegistry('https://swkazurecr.azurecr.io', 'acr-credential') {
            # env.BUILD_NUMBER는 Jenkins의 기본 환경변수로 현재 Jenkins의 빌드 번호를 의미
            # push()안에 있는 번호로 이미지 태그가 지정되어 ACR로 Push
            app.push("${env.BUILD_NUMBER}")
        }
    }
}
