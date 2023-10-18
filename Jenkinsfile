node {
  def GIT_URL = 'https://github.com/shashikanthmartha/Setup-CI-CD-with-Github-Jenkins-Maven-and-Tomcat-on-AWS.git'
  
  
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
    /*
      stage('Quality Gate') {
        timeout(time: 1, unit: 'HOURS') {
            echo 'status success'
            def qg = waitForQualityGate()
            if (qg.status != 'OK') {
                echo 'status success'
                error "Pipeline aborted due to quality gate failure: ${qg.status}"
            }
        }
    }*/


    stage('Deploy to Tomcat') {

      
      deploy adapters: [tomcat9(credentialsId: '8fb82e3b-3b73-400b-8a75-dfaa86afa0b0', path: '', url: 'http://localhost:8081')], contextPath: 'maven1', war: '**/*.war'
      

      
    }

    echo "deployed to tomcat"
    
  
}
