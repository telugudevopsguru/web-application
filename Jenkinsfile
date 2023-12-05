pipeline {
    agent any
    
    tools{
        maven "Maven-3.9.5"
    }



	  options {
  
// keep the latest last 10 builds of this pipeline 
buildDiscarder(logRotator(numToKeepStr: '2'))

// Disable the concurrent builds
    disableConcurrentBuilds()

// Add the timestamps to the build logs
    timestamps()
  }


    stages {
        stage('Clone the Repository') {
            steps {
              git credentialsId: 'GitHub-credential', url: 'https://github.com/telugudevopsguru/web-application.git'
            }
        }
        
      stage('Build the code') {
            steps {
             sh 'mvn clean package'
                
            }
        }
          
          stage('Pushthe artifacts to Jfrog artifaciory'){
              steps
              {
                  nexusArtifactUploader artifacts: [[artifactId: 'web-applications', classifier: '', file: '/var/lib/jenkins/workspace/sample-jenkins-pipeline/target/web-application.war', type: 'war']], credentialsId: 'nexus-cred', groupId: 'com.gantasoft', nexusUrl: '18.206.161.71:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'
              }
          }
        stage('Deploy to tomcat'){
              steps
              {
                 deploy adapters: [tomcat7(credentialsId: 'Tomcat-credentials ', path: '', url: 'http://34.236.158.140:8080')], contextPath: null, war: '**/*.war'
                  
              }
          }
        
        
    }
    post {
  always {
    slackSend channel: 'jenkins-integartion', message: 'Build completed', teamDomain: 'telugudevopsguru', tokenCredentialId: 'Slack-Notification'
      
  }
}
}
