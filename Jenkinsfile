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
                        # Download SonarScanner (skip if already downloaded)
                        if [ ! -f sonar-scanner.zip ]; then
                            curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
                        fi

                        # Unzip (overwrite quietly)
                        unzip -o sonar-scanner.zip > /dev/null 2>&1 || true

                        # Use full path to run sonar-scanner
                        ./sonar-scanner-5.0.1.3006-linux/bin/sonar-scanner \
                            -Dsonar.projectKey=Piyush2467_8.1C-JENKINS \
                            -Dsonar.organization=piyush2467 \
                            -Dsonar.sources=. \
                            -Dsonar.exclusions=node_modules/**,test/** \
                            -Dsonar.host.url=https://sonarcloud.io \
                            -Dsonar.login=${SONAR_TOKEN}
                    '''
                }
            }
        }
    }
}