pipeline {
    agent any
    environment {
        maven = "/usr/share/maven"
        java  = "/usr/share/java"
    }
    stages{
        stage("Git"){
            steps{
                git branch: 'main', url: 'https://github.com/surendraboyapati/spring-petclinic.git'
                }
        }
        stage("Package"){
            steps{
                sh '''./mvnw -s settings.xml package'''
            }
        }
        stage("Deploy jar file locally(by killing previous deployment)"){
            steps{
                // while using  must have visudo permissions and restart jenkins..
                // jenkins ALL=(ALL) NOPASSWD: ALL
                sh '''
                sudo chmod +x script.sh
                sudo ./script.sh'''
            }
        }

        stage("mvn deploy"){
            steps{
                sh '''./mvnw -s settings.xml deploy'''
            }
        }
        stage("Push Artifact"){
            steps{
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: 'spring-petclinic',
                        classifier: '', 
                        file: 'target/spring-petclinic-2.5.0-SNAPSHOT.jar', 
                        type: 'jar'
                    ]
                ],
                
                 credentialsId: 'nexuslogin', 
                 groupId: 'org.springframework.boot', 
                 nexusUrl: '52.190.56.78:8081', 
                 nexusVersion: 'nexus3', 
                 protocol: 'http', 
                 repository: 'maven-snapshots', 
                 version: '2.5.0-SNAPSHOT'
            }
        }
    }
}
