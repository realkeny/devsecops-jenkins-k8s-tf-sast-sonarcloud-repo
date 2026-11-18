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
				sh '''
				mvn clean verify sonar:sonar -Dsonar.organization=realkenydevsecopsapp -Dsonar.projectKey=realkenydevsecopsapp -Dsonar.host.url="https://sonarcloud.io" -Dsonar.token=f6b230bd29ffc1997acc91cdf25a9f3b826a440b
				'''
			}
		}
	}
  }
}
