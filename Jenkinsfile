pipeline { 
    agent any
    ENVIRONMENT {
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
                    SONAR_HOME/bin/sonar-scanner \
                    -Dsonar.projectName=Mern-Project \
                    -Dsnar.projectkey=mern-project \
                    -Dsonar.sources-. 
                    '''
                }
            }
        }
        stage("sonar quality gate") {
            steps {
                echo "Waiting for Sonarqube quality gate..."
                timeout(time: 4, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: False
                }
            }
        }
        stage("deploy") {
            stps {
                sh 'docker compose up -d'
            }
        }

    }
}