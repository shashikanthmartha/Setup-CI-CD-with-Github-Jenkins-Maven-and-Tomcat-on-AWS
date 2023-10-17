node {
  def GIT_URL = 'https://github.com/shashikanthmartha/Setup-CI-CD-with-Github-Jenkins-Maven-and-Tomcat-on-AWS.git'
  

  try {
    stage('Checkout') {
      echo " $GIT_URL  git url"
      checkout([$class: 'GitSCM', branches: [
        [name: '*/master']
      ], userRemoteConfigs: [
        [url: GIT_URL]
      ]])
    }
    echo "checkout completed"

    stage('Build') {
      bat "mvn clean package"

    }
    echo "build success"

    stage('Static Code Analysis') {
    withSonarQubeEnv('Sonar') {
    // Use the SonarQube credentials and server configuration defined in Jenkins
    bat "mvn sonar:sonar"
        }
    }
    echo " sonarqube"
    stage('Deploy to Tomcat') {
      echo 'entered into the deploy stage'

          deploy adapters: [tomcat9(credentialsId: '8fb82e3b-3b73-400b-8a75-dfaa86afa0b0', path: '', url: 'http://localhost:8081')], contextPath: 'maven1', war: '**/*.war'
     
      echo 'tomcat credentials success '
     
      echo "deploy to tomcat"
  } catch (Exception e) {
    echo 'Exception e'
    currentBuild.result = 'FAILURE'

    throw e
  }

}
