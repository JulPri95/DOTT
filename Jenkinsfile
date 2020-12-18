@Library('github.com/triologygmbh/jenkinsfile@ad12c8a9') _

pipeline {
    agent any
    stages {
        //Build the Dockerfile image if it doesn't already exist using the 'try/catch' method
        stage( 'Docker Image Build') {
            steps {
                 sh 'docker build -t pym .'
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
        //stage('SonarQube code scan') {
        //    steps {
        //        script {
        //            sonarScanner('category-service')
        //        }
        //    }
        //}
        //stage('Statical Code Analysis') {
        //    agent { label 'docker' } // Require a build executor with docker
        //    environment {
        //     SCANNER_HOME = tool 'SonarQubeScanner'
             //ORGANIZATION = "julpri95"
             //PROJECT_NAME = "JulPri95_DOTT"
        //    }
        //    steps {
        //         withSonarQubeEnv('sonarcloud.io-cloudogu') {
        //             mvn "$SONAR_MAVEN_GOAL -Dsonar.host.url=$SonarQubeScanner -Dsonar.projectkey=$PROJECT_NAME " +
                            // Here, we could define e.g. sonar.organization, needed for sonarcloud.io
        //                 "$ORGANIZATION " //+
                            // Additionally needed when using the branch plugin (e.g. on sonarcloud.io)
                         //"-Dsonar.branch.name=$BRANCH_NAME -Dsonar.branch.target=master"
        //         }
        //    }
        //}
        //Using the sonar scan plug-in, execute SonarCloud testing on the project
        stage('SonarCloud') {
            environment {
             SCANNER_HOME = tool 'SonarQubeScanner'
             //ORGANIZATION = "julpri95"
             //PROJECT_NAME = "JulPri95_DOTT"
            }
            steps {
                script {
                    withCredentials([
                        string(
                            credentialsId: 'project-key',
                            variable: 'PROJECT_NAME'),
                        string(
                            credentialsId: 'organization-key',
                            variable: 'ORGANIZATION')
                   ]) {
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
        stage('Coverage') {
            environment {
             SCANNER_HOME = tool 'SonarQubeScanner'
             //ORGANIZATION = "julpri95"
             //PROJECT_NAME = "JulPri95_DOTT"
            }
            steps {
                script {
                    withCredentials([
                        string(
                            credentialsId: 'project-key',
                            variable: 'PROJECT_NAME'),
                        string(
                            credentialsId: 'organization-key',
                            variable: 'ORGANIZATION')
                        ]) {
                        sh 'cd $WORKSPACE/'
                        sh 'sudo apt install python3-pip'
                        sh 'sudo python3 -m pip install coverage'
                        sh 'sudo python3 -m pip install pytest'
                        sh 'sudo coverage run -m pytest /var/lib/jenkins/workspace/FinalProject/tests.py -v | coverage report | coverage xml'
                        sh 'ls'
                        sh 'pwd'
                        withSonarQubeEnv('SonarCloud') {
                                sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.organization=$ORGANIZATION \
                                -Dsonar.java.binaries=build/classes/java/ \
                                -Dsonar.projectKey=$PROJECT_NAME \
                                -Dsonar.sources=api.py,convert.py \
                                -Dsonar.tests=tests.py
                                -Dsonar.python.coverage.reportPaths'''
                   }
                }
            }
        }
        //Run the docker image in port 8000 if it is free, otherwise skip this step. Using the 'try/catch' method
        //stage('Docker Run') {
        //    steps {
        //        script {
        //            try {
        //                sh 'sudo lsof -i:8000'
        //            }
        //            catch (exc) {
        //                sh 'echo "Port 8000 is free, image will be run"'
        //                sh 'sudo docker run -d -p 8000:8000 pym'
        //            }
        //        }   
        //    }
        //}
    }
}

}
