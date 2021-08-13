pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

        // Stage3 : Publish the source code to Sonarqube
        stage ('Sonarqube Analysis'){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'VinayDevOpsLab', classifier: '', file: 'target/VinayDevOpsLab-0.0.3-SNAPSHOT.war', type: 'war']], credentialsId: '53d1fa91-76b6-45dc-b698-3a277146d5dc', groupId: 'com.vinaysdevopslab', nexusUrl: '172.20.10.133:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'VinaysDevOpsLab-SNAPSHOT', version: '0.0.3-SNAPSHOT'
            }
        }       
        
    }

}
