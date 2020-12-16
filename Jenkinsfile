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
       // stage('Docker Build') {
       //     environment {
       //         PORT_ACTIVE = sh(returnStdout: true, script: 'sudo lsof -i:8000')
       //     }
       //     when {
       //         expression { env.PORT_ACTIVE = null }
       //     }
       //     steps {
       //         sh 'echo "$PORT_ACTIVE"'
       //         sh 'sudo docker build -t pym . '
       //         sh 'sudo docker run -d -p 8000:8000 pym'
       //     }
       // }
        stage('Unit Testing') {
            environment {
                PORT_ACTIVE = ' '
                PORT_IS_ACTIVE = sh(returnStdout: true, script: 'sudo lsof -i:8000')
            }
            when {
                expression { env.PORT_ACTIVE = null }
            }
            steps { 
                sh 'echo "$PORT_IS_ACTIVE"'
                sh 'sudo docker build -t pym . '
                sh 'sudo docker run -d -p 8000:8000 pym'    
                //sh 'cd /home/cloud_user/DOTT'
                //sh 'python tests.py'
            }
        }
    }
}
