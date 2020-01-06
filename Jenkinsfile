pipeline {
	agent any
	parameters
	{
		booleanParam(name: 'Release', defaultValue: false, description: 'Will Push code to the Server')
	}
	stages {
		stage('Source') { 
			steps {
				checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/prabhavagrawal/discoveri-happytrip.git']]])
			}
		}
		stage('Build') { 
			tools {
				jdk 'jdk8'
				maven 'maven'
			}
			steps {
				powershell 'java -version'
				powershell 'mvn -version'
				powershell 'mvn clean package'

			}
		}
		stage('Archive'){
			steps{
				archiveArtifacts '**/*.war'
			}
		}
		
		stage('Mail'){
			steps{
				//mail bcc: '', body: 'Hello All', cc: '', from: '', replyTo: '', subject: 'DEPLOY BUILD', to: 'moniphukan6@gmail.com'
				mail bcc: '', body: 'Hello All', cc: '', from: '', replyTo: '', subject: 'DEPLOY BUILD', to: 'moniphukan6@gmail.com'
			}
		}
		stage('Approval'){
			when{
				expression {params.Release == true}
			}
			steps{
				script {
					input message: 'Approve Deploy?', ok: 'Yes', submitter: 'PME'
				} 
  			}
		}
		stage('Deploy') {
			when{
				//when the release parmeter is true only then the release will be given
				expression {params.Release == true}
			}
			steps{
				echo "Deploying"
				//deploy adapters: [tomcat7(credentialsId: '98e9cbd9-106c-4efa-8238-9888f9bc8fc3', path: '', url: 'http://localhost:8085')], contextPath: 'happytrip', war: '**/*.war'
				deploy adapters: [tomcat7(credentialsId: 'e2df397d-1c2e-44f6-9ce5-c09d32e7f093', path: '', url: 'http://localhost:8085')], contextPath: 'MyHappyTrip', war: '**/*.war'
			}
		}
	}
}
