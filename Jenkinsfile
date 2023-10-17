node {
  def GIT_URL = 'https://github.com/shashikanthmartha/Setup-CI-CD-with-Github-Jenkins-Maven-and-Tomcat-on-AWS.git'
  def SONARQUBE_SERVER = 'http://localhost:9000'

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

      echo ' enter into the stage steps'
      deploy adapters: [tomcat9(credentialsId: '8fb82e3b-3b73-400b-8a75-dfaa86afa0b0', path: '', url: 'http://localhost:8081')], contextPath: 'maven1', war: '**/*.war'
      //deploy adapters: [tomcat9(credentialsId: '8fb82e3b-3b73-400b-8a75-dfaa86afa0b0', path: '', url: 'http://localhost:8081')], contextPath:maver , war: '**/*.war'
      // def TOMCAT_URL = 'http://65.1.95.121:9090'
      // def TOMCAT_USERNAME = credentials('tomcat')
      // def TOMCAT_PASSWORD = credentials('s3cret')
      echo 'tomcat credentials success '
      /* bat ("Copy-Item -Path “ C:\\Users\Administrator\.jenkins\workspace\maven_project\webapp\target\webapp.war ” -Destination “C:\\Users\Administrator\.jenkins\workspace\maven_project\webapp\target\webapp.war” -Recurse")
       */
      /* def sourcePath = Paths.get('C://Users/Administrator/.jenkins/workspace/maven_project/webapp/target/webapp.war')
       def destinationPath = Paths.get('C://Users/Administrator/.jenkins/workspace/maven_project/webapp/target/webapp.war')
       echo 'paths defined'
        bat Files.copy(sourcePath , destinationPath )
        */
      echo 'copy success'

      /*bat "curl --upload-file C:/Users/Administrator/.jenkins/workspace/maven_project/webapp/target/webapp.war %TOMCAT_USERNAME%:%TOMCAT_PASSWORD%@%TOMCAT_URL%/manager/text/deploy?path=/your-app&update=true"
              // Define the paths to your Tomcat and web application (WAR file)
      def TOMCAT_PATH = 'C:/Users/Administrator/Downloads/apache-tomcat-9.0.80-windows-x64/apache-tomcat-9.0.80/'
      def WAR_FILE = 'C:/Users/Administrator/.jenkins/workspace/maven_project/webapp/target/webapp.war'

       // Stop Tomcat
      bat "cd ${tomcatPath}\\bin && shutdown.bat"

       // Remove the existing web application (if any)
      bat "del ${tomcatPath}\\webapps\\${warFile}"

        // Deploy the new web application
      bat "copy target\\${warFile} ${tomcatPath}\\webapps\\"

       // Start Tomcat
      bat "cd ${tomcatPath}\\bin && startup.bat" */

    }

    /* stage('Deploy to Tomcat') {
          bat "copy webapps/target/webapp.war CATALINA_HOME/webapps"
      } */
    echo "deploy to tomcat"
  } catch (Exception e) {
    echo 'Exception e'
    currentBuild.result = 'FAILURE'

    throw e
  }

}

