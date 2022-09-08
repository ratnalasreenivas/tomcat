pipeline{
    agent {label 'dev'}
    tools{
maven 'maven-3.8.6'
}
    
    stages{
        stage('Git Checkout')
        {
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: '30afe31a-dec4-4371-8bb2-8e446a6f762f', url: 'https://github.com/PMFayazAhmed/corelogic.git']]])
                
            }
        }
        stage('Message')
        {
            steps{
                echo 'Welcome to DEVOPS TEAM'
            }
        }
        
         stage('Maven Build')
        {
            steps{
                sh 'mvn package -f pom.xml'
            }
        }
         stage('Nexus Upload')
        {
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'corelogic', classifier: '', file: 'target/Mymaster-corelogic.war', type: 'war']], credentialsId: 'ac178314-d8bc-4c9f-b1c7-69cb1a9697a5', groupId: 'com.adaequare', nexusUrl: '34.125.4.65:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'
            }
        }
        
    }
}
