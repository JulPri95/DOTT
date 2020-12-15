pipeline {
    agent any
    stages {
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
        stage('Unit Testing') {
            steps {
                sh 'sudo docker build -t pym . '
                sh 'sudo docker run -p 8000:8000 pym'
                sh 'cd /home/cloud_user/DOTT'
                sh 'python tests.py'
            }
        }
    }
}
