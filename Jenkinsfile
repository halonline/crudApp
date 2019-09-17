node
{
    stage ('scm-cheoukout'){
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Thirumaleshwar/crudApp.git']]])
    }
    stage ('compile using maven'){
        withSonarQubeEnv('sonar')
         {
        def maven = tool name: 'Maven', type: 'maven'
        sh "${maven}/bin/mvn clean package sonar:sonar"
         }
    }
    stage("Wait for Sonar Quality Gate Response"){
            timeout(time: 1, unit: 'HOURS') {
              def qualityGate = waitForQualityGate()
              if (qualityGate.status != 'OK') {
                error "Pipeline aborted due to quality gate failure: ${qualityGate.status}"
              }
            }
        }
   stage('Build Docker Image') {
	    		sh 'docker image build -t thirumaleshwarbudamala/admin-service:${BUILD_NUMBER} .'
	    		sh 'docker tag thirumaleshwarbudamala/admin-service:${BUILD_NUMBER} thirumaleshwarbudamala/admin-service:latest' 
	    		//sh 'docker container run -d --name idexceldemo27 -p 808:8080 thirumaleshwarbudamala/admin-service:${BUILD_NUMBER}'
	    }
   stage('Push Docker Image to Docker Hub') {
            sh 'docker login -u thirumaleshwarbudamala -p Thiru@2701'
            sh 'docker push thirumaleshwarbudamala/admin-service:latest'
        }
    

    
}
