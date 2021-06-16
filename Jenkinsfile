pipeline
{
  agent any
  
  environment {
    DEPLOY_CREDS = credentials('deploy-anypoint-user')
    MULE_VERSION = '4.3.0'
    WORKER = "Micro"
  }
  
  stages{
    stage('Build Application'){
      steps{
    		sh 'mvn clean install -DskipTests'
    	}
    }
    
    stage('Deploy Application to MuleSoft CloudHub'){
     environment {
        ENVIRONMENT = 'Sandbox'
        APP_NAME = 'NRE-trainDelayServices'
      }
     steps{
        script {
          withCredentials([
            usernamePassword(credentialsId: 'deploy-anypoint-user',
              usernameVariable: 'username',
              passwordVariable: 'password')
          ]) {
            print 'username=' + username + 'password=' + password
            }
             print %MULE_VERSION% 
             sh 'mvn package deploy -DmuleDeploy -DskipTests -Dmule.version="%MULE_VERSION%" -Danypoint.username=username -Danypoint.password=password -Dcloudhub.app="%APP_NAME%" -Dcloudhub.environment="%ENVIRONMENT%" -Dcloudhub.worker="%WORKER%"'
          }
    	}
    }
    
    stage('Perform Regression Testing'){
      steps{
    		sh 'newman run /Users/rm/Desktop/NjcLabs/newman/getSoapDetails.postman_collection.json'
    	 }
    }
  }
}