pipeline {
    agent any
    stages {
        stage('Unit Testing') {
            steps {
                sh 'sudo -s docker build -t pym . '
                sh 'sudo -s docker run -ti -p 8000:8000 pym'
                sh 'python test.py'
            }
        }
        stage('SonarCloud') {
            environment {
             SCANNER_HOME = tool 'SonarQubeScanner'
             ORGANIZATION = "julpri95"
             PROJECT_NAME = "JulPri95_DOTT"
            }
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.organization=$ORGANIZATION \
                    -Dsonar.java.binaries=build/classes/java/ \
                    -Dsonar.projectKey=$PROJECT_NAME \
                    -Dsonar.sources=.'''
                }
            }
        }
    }
}
