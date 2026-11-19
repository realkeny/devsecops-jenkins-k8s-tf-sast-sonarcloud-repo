pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: maven
            image: maven:3.9-eclipse-temurin-17
            command:
            - cat
            tty: true
        '''
    }
  }
  stages {
    stage('CompileandRunSonarAnalysis'){
		steps{
			container('maven') {
				withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]){
					sh '''
					mvn clean verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.organization=realkenydevsecopsapp -Dsonar.projectKey=realkenydevsecopsapp -Dsonar.host.url="https://sonarcloud.io" -Dsonar.token=$SONAR_TOKEN
					'''
				}
			}
		}
	}
	stage('RunSCAAnalysisUsingSnyk'){
		steps{
			container('maven'){
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]){
					sh 'mvn snyk:test -fn'
				}
			}
		}
	}
  }
}
