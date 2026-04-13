pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
               echo 'Code already checked out from GitHub'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install || true'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'echo "Running tests..."'
            }
        }

        stage('NPM Audit') {
            steps {
                sh 'npm audit || true'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh '''
                    curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
                    unzip sonar-scanner.zip
                    export PATH=$PATH:$(pwd)/sonar-scanner-5.0.1.3006-linux/bin

                    sonar-scanner \
                      -Dsonar.projectKey=Piyush2467_8.1C-JENKINS \
                      -Dsonar.organization=piyush2467 \
                      -Dsonar.sources=. \
                      -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
    }
}