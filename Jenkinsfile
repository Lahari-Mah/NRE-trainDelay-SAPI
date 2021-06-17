pipeline
{
  agent any
  
  environment {
    DEPLOY_CREDS = credentials('deploy-anypoint-user')
    MULE_VERSION = '4.3.0'
    WORKER = "Micro"
  }
  
  options {
      timeout(time: 1, unit: 'HOURS') 
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
             sh 'echo $DEPLOY_CREDS_USR env $ENVIRONMENT'
             
             timeout(time: 1, unit: 'HOURS'){
              sh 'mvn package deploy -DmuleDeploy -DskipTests -Dmule.version=$MULE_VERSION -Danypoint.username=$DEPLOY_CREDS_USR -Danypoint.password=$DEPLOY_CREDS_PSW -Dcloudhub.app=$APP_NAME -Dcloudhub.environment=$ENVIRONMENT -Dcloudhub.worker=$WORKER -Dmaven.properties=src/main/resources/cloudhubdev.properties'
    	    }
    	}
    }
    
    stage('Perform Regression Testing'){
      steps{
    		sh 'newman run /Users/rm/Desktop/NjcLabs/newman/getDBSoapDetails.postman_collection.json'
    	 }
    }
  }
}