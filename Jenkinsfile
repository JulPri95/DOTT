pipeline {
    agent any
    stages {
        stage('SCM') {
            steps {
                git url: 'https://github.com/JulPri95/DOTT.git'
            }
        }
        stage('SonarCloud') {
            environment {
             SCANNER_HOME = tool 'SonarQubeScanner'
             ORGANIZATION = "julpri95-github"
             PROJECT_NAME = "julpri95_dott"
            }
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.organization=$ORGANIZATION \
                    -Dsonar.java.binaries=build/classes/java/ \
                    -Dsonar.projectKey=$PROJECT_NAME \
                    -Dsonar.sources=.'''
             }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }
      }
   }
}
