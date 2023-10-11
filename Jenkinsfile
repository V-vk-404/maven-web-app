node{

    stage('Clone repo'){
        git 'https://github.com/V-vk-404/maven-web-app.git'
    }
    
    stage('Maven Build'){
        sh 'mvn clean package'
    }
    
    stage('SonarQube analysis') {       
        withSonarQubeEnv('sq1') {
       	sh 'mvn sonar:sonar'    	
               }
    }
    stage('Nexus Artifact upload'){
        nexusArtifactUploader artifacts: [[artifactId: '01-maven-web-app', classifier: '', file: 'target/01-maven-web-app.war', type: 'war']], credentialsId: 'Nexus-key', groupId: 'in.ashokit', nexusUrl: '3.145.218.53:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-app', version: '3.0-SNAPSHOT'
    }
    stage('Code Deploy'){
        sshagent(['tomcat-server']) {
           sh 'scp -o StrictHostKeyChecking=no target/01-maven-web-app.war ubuntu@18.217.111.240:/opt/apache-tomcat-8.5.87/webapps'
      }
    }
} 
