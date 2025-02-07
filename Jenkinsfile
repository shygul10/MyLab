pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    environment{
       ArtifactId = readMavenPom().getArtifactId()
       Version = readMavenPom().getVersion()
       Name = readMavenPom().getName()
       GroupId = readMavenPom().getGroupId()
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

        // Stage3 : Publish the artifacts to Nexus
        stage ('Publish to Nexus'){
            steps {
                script {

                def NexusRepo = Version.endsWith("SNAPSHOT") ? "VinaysDevOpsLab-SNAPSHOT" : "VinaysDevOpsLab-RELEASE"

                nexusArtifactUploader artifacts: 
                [[artifactId: "${ArtifactId}", 
                classifier: '', 
                file: "target/${ArtifactId}-${Version}.war", 
                type: 'war']], 
                credentialsId: '53d1fa91-76b6-45dc-b698-3a277146d5dc', 
                groupId: "${GroupId}", 
                nexusUrl: '172.20.10.133:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: "${NexusRepo}", 
                version: "${Version}"
             }
            }
        }   
        
         // Stage 4 : Print some information
        stage ('Print Environment variables'){
                    steps {
                        echo "Artifact ID is '${ArtifactId}'"
                        echo "Version is '${Version}'"
                        echo "GroupID is '${GroupId}'"
                        echo "Name is '${Name}'"
                    }
                }
        
        //stage 5 : Deploying
        stage('Deploy'){
            steps {
                echo "Deploying ...."
                sshPublisher(publishers: [sshPublisherDesc(configName: 
                                                           'Ansible_Controleur', 
                                                           transfers: [
                                                               sshTransfer(cleanRemote: false, 
                                                                                   excludes: '', 
                                                                                   execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy.yaml -i /opt/playbooks/hosts', 
                                                                                   execTimeout: 120000, 
                                                                                   flatten: false, 
                                                                                   makeEmptyDirs: false, 
                                                                                   noDefaultExcludes: false, 
                                                                                   patternSeparator: '[, ]+', 
                                                                                   remoteDirectory: '', 
                                                                                   remoteDirectorySDF: false, 
                                                                                   removePrefix: '', sourceFiles: '')], 
                                                           usePromotionTimestamp: false, 
                                                           useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }

}
