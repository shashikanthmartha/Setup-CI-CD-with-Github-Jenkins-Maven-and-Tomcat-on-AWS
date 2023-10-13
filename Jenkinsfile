pipeline{
    agent any
    tools{
        maven 'Maven'
    }
    stages{
        stage ('Build'){
            steps{
                bat 'mvn clean package'
            }
            post{
                success{
                    echo "Archiving the Artifacts"
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to tomcat server') {
            steps{
                script{
                    readProp = readProperties file: 'build.properties'
                }
                echo "This is running on ${readProp['deploy.type']}"
                deploy adapters: [tomcat9(credentialsId: '3e5e5fb9-73e4-4243-840a-b0ceaa706c00', path: '', url: 'http://65.1.95.121:9090/')], contextPath: null, war: '**/*.war'
            }
        }
    }
}
