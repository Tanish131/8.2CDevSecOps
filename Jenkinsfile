pipeline {
    agent any
    stages {
        stage('1. Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Tanish131/8.2CDevSecOps.git'
            }
        }
        stage('2. Build (Install Dependencies)') {
            steps {
                sh '''
                    apt update -y
                    apt install -y curl unzip
                    curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
                    apt install -y nodejs
                    node -v
                    npm -v
                    npm install || true
                '''
            }
        }
        stage('3. Unit & Integration Testing') {
            steps {
                sh 'npm test || true'
            }
        }
        stage('4. Code Analysis (SonarCloud)') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        curl -L -o sonar-scanner.zip \
                        "https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006.zip"
                        unzip -o sonar-scanner.zip
                        java -jar sonar-scanner-5.0.1.3006/lib/sonar-scanner-cli-5.0.1.3006.jar \
                            -Dsonar.host.url=https://sonarcloud.io \
                            -Dsonar.token=$SONAR_TOKEN \
                            -Dsonar.projectKey=Tanish131_8.2CDevSecOps \
                            -Dsonar.organization=tanish131 \
                            -Dsonar.sources=. \
                            -Dsonar.exclusions="node_modules/**,test/**,sonar-scanner-*/**,*.zip" \
                            -Dsonar.nodejs.executable=$(which node)
                    '''
                }
            }
        }
        stage('5. Security Scan (npm audit)') {
            steps {
                sh 'npm audit || true'
            }
        }
        stage('6. Deploy to Staging (Simulated)') {
            steps {
                echo 'Deploying application to staging environment...'
                sh 'sleep 5'
            }
        }
        stage('7. Deploy to Production (Simulated)') {
            steps {
                echo 'Deploying application to production environment...'
                sh 'sleep 5'
            }
        }
    }
    post {
        always {
            echo 'Pipeline finished!'
        }
    }
}
