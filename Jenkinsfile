pipeline {

  agent any
  environment {
    //adding a comment for the commit test
    DEPLOY_CREDS = credentials('anypoint.credentials')
    MULE_VERSION = '4.4.0'
    BG = '62247c48-fb85-4268-a41f-780033385d51'
    WORKER = 'Micro'
    M2SETTINGS = '/var/lib/jenkins/workspace/settings.xml'
    
  }
  stages {
    stage('Build') {
      steps {
            sh 'mvn -B -U -e -V clean -gs $M2SETTINGS -DskipTests package'
      }
    }

    stage('Test') {
      steps {
          sh 'mvn test'
      }
    }

     stage('Deploy Development') {
      environment {
        ENVIRONMENT = 'Design'
        APP_NAME = 'omni-channel-api-v1'
      }
      steps {
            sh 'mvn -U -V -e -B -gs $M2SETTINGS -DskipTests deploy -DmuleDeploy -Dmule.version=$MULE_VERSION -Danypoint.username=$DEPLOY_CREDS_USR -Danypoint.password=$DEPLOY_CRED_PSW -Dcloudhub.app=$APP_NAME -Dcloudhub.environment=$ENVIRONMENT -Dcloudhub.bg=$BG -Dcloudhub.worker=$WORKER'
      }
    }
    stage('Deploy Production') {
      environment {
        ENVIRONMENT = 'Sandbox'
        APP_NAME = 'omni-channel-api-v1'
      }
      steps {
            sh 'mvn -U -V -e -B -gs $M2SETTINGS -DskipTests deploy -DmuleDeploy -Dmule.version=$MULE_VERSION -Danypoint.username=$DEPLOY_CREDS_USR -Danypoint.password=$DEPLOY_CRED_PSW -Dcloudhub.app=$APP_NAME -Dcloudhub.environment=$ENVIRONMENT -Dcloudhub.bg=$BG -Dcloudhub.worker=$WORKER'
      }
    }
  }
}
