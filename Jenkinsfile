pipeline {

  agent any
  environment {
    //adding a comment for the commit test
    DEPLOY_CREDS = credentials('anypointcredentials')
    MULE_VERSION = '4.4.0'
    BG = 'VST'
    WORKER = 'Micro'
 //   M2SETTINGS = '$HOME/opt/maven/apache-maven-3.8.6/.m2/settings.xml'
    
  }
  stages {
    stage('Build') {
      steps {
            sh 'mvn -B -U -e -V clean -gs /var/lib/jenkins/workspace/settings.xml -DskipTests package'
      }
    }

    stage('Test') {
      steps {
          sh "mvn test"
      }
    }

     stage('Deploy Development') {
      environment {
        ENVIRONMENT = 'Sandbox'
        APP_NAME = 'omni-channel-api-v1'
      }
      steps {
            sh 'mvn -U -V -e -B -gs /var/lib/jenkins/workspace/settings.xml -DskipTests deploy -DmuleDeploy -Dmule.version="%MULE_VERSION%" -Danypoint.username="%DEPLOY_CREDS%" -Danypoint.password="%DEPLOY_CREDS%" -Dcloudhub.app="%APP_NAME%" -Dcloudhub.environment="%ENVIRONMENT%" -Dcloudhub.bg="%BG%" -Dcloudhub.worker="%WORKER%"'
      }
    }
    stage('Deploy Production') {
      environment {
        ENVIRONMENT = 'Design'
        APP_NAME = 'omni-channel-api-v1'
      }
      steps {
            sh 'mvn -U -V -e -B -gs /var/lib/jenkins/workspace/settings.xml -DskipTests deploy -DmuleDeploy -Dmule.version="%MULE_VERSION%" -Danypoint.username="%DEPLOY_CREDS%" -Danypoint.password="%DEPLOY_CREDS%" -Dcloudhub.app="%APP_NAME%" -Dcloudhub.environment="%ENVIRONMENT%" -Dcloudhub.bg="%BG%" -Dcloudhub.worker="%WORKER%"'
      }
    }
  }
}
