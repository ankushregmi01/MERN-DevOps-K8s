pipeline { 
    agent any
    environment {
        SONARQUBE_ENV = 'Sonar'
    }
    stages {
        stage("build") {
            steps {
                echo "Building project..."
                sh 'docker compose build'
            }
        }
        stage("Sonarqubbe Analysis") {
            steps {
                echo "running Sonarqube analysis..."
                withSonarQubeEnv('Sonar') {
                    sh '''
                    sonar-scanner \
                    -Dsonar.projectName=Mern-Project \
                    -Dsonar.projectKey=mern-project \
                    -Dsonar.sources=. 
                    '''
                }
            }
        }
        stage("sonar quality gate") {
            steps {
                echo "Waiting for Sonarqube quality gate..."
                timeout(time: 4, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: false
                }
            }
        }
        stage("deploy") {
            steps {
                sh 'docker compose up -d'
            }
        }

    }
}