pipeline {
    agent any
    stages {
        stage('SCM') {
            steps {
                git url: 'https://github.com/JulPri95/DOTT.git'
            }
        }
        stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                   
                    }
                }
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
