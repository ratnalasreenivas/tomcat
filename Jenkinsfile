pipeline{
    agent any
    tools{
        maven 'MAVEN_HOME'
    }
    
      parameters { choice(name: 'Environment', choices: ['staging', 'preprod', 'prod'], description: 'Profile needs to be used while executing test') }
      
    stages{

        stage ('Git CheckOut'){ 
            steps {
               //define the single or multiple step
                bat 'echo Git Checkout'
                checkout([$class: 'GitSCM', branches: [[name: 'actual']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/PMFayazAhmed/corelogic.git']]])
            }
        }
        stage ('Restore Packages'){
            steps {
                //define the single or multiple step
                 bat 'echo Restore Package'
            }
        }
        stage ('Build'){
            steps {
               //define the single or multiple step
                bat 'echo Build'
                bat 'mvn clean package'
            }
        }
        stage ('Nexus Upload'){
            steps {
                bat 'echo Uploading the artifact..'
                nexusArtifactUploader artifacts: [[artifactId: 'karateframework', classifier: '', file: 'target/karateframework-0.0.1-SNAPSHOT.war', type: 'war']], credentialsId: 'd00789b3-9efa-4ffb-b2e9-5efd19314051', groupId: 'com.api.automation', nexusUrl: 'localhost:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '0.0.1-SNAPSHOT'
            }
        }
                stage ('Deploy to tomcat'){
            steps {
                bat 'echo Deploying the application..'
                deploy adapters: [tomcat9(credentialsId: 'b453bbf4-58af-4d2c-81f7-5a3814b4aa33', path: '', url: 'http://localhost:8090/')], contextPath: 'Karateframework', war: 'target/karateframework-0.0.1-SNAPSHOT.war'
            }
        }


        stage ('Run the Test') {
            steps {
                 //define the single or multiple step
                catchError(buildResult: 'SUCCESS',stageResult: 'FAILURE'){
                bat 'echo Test Execution Started'
                //bat 'mvn -P %Environment% test'
                //bat 'mvn -P %Environment% compile test'
                bat 'mvn test'
            }
        }
        }

}
    post {
        always {
            junit 'target/surefire-reports/*.xml'
            cucumber failedFeaturesNumber: -1, failedScenariosNumber: -1, failedStepsNumber: -1, fileIncludePattern: '*/.json', jsonReportDirectory: 'target/surefire-reports', pendingStepsNumber: -1, reportTitle: 'Karate Test Execution', skipEmptyJSONFiles: true, skippedStepsNumber: -1, sortingMethod: 'ALPHABETICAL', undefinedStepsNumber: -1
        }

    }

}
