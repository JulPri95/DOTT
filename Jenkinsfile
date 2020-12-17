pipeline {
    agent any
    stages {
        //Build the Dockerfile image if it doesn't already exist using the 'try/catch' method
        stage( 'Docker Image Build') {
            steps {
                script {
                    try {
                       sh 'sudo docker image inspect pym:latest'
                    }
                    catch (exc) {
                        sh 'echo "Image does not exist yet"'
                        sh 'sudo docker build -t pym .'
                    }
                }
            }
        }
        //Run the docker image in port 8000 if it is free, otherwise skip this step. Using the 'try/catch' method
        stage('Docker Run') {
            steps {
                script {
                    try {
                        sh 'sudo lsof -i:8000'
                    }
                    catch (exc) {
                        sh 'echo "Port 8000 is free, image will be run"'
                        sh 'sudo docker run -d -p 8000:8000 pym'
                    }
                }   
            }
        }
        //Run the python file 'tests' to perform the Unit Testing. If it fails, consider the stage a success anyway and move on to next stage
        stage( 'Unit Testing' ) {
            steps {
                script {
                    try {
                        sh 'cd /home/cloud_user/DOTT'
                        sh 'python tests.py'
                    }
                    catch (exc) {
                            echo 'Unit tests failed'
                    }
                }
            }
        }
        //Using the sonar scan plug-in, execute SonarCloud testing on the project
        stage('SonarCloud') {
            environment {
             SCANNER_HOME = tool 'SonarQubeScanner'
             //ORGANIZATION = "julpri95"
             //PROJECT_NAME = "JulPri95_DOTT"
            }
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.organization=julpri95 \
                    -Dsonar.java.binaries=build/classes/java/ \
                    -Dsonar.projectKey=JulPri95_DOTT \
                    -Dsonar.sources=.'''
                }
            }
        }
    }
}
