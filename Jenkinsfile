pipeline {
    agent any

    parameters {
        string(name: 'ENV', defaultValue: 'dev', description: 'Environment to deploy')
        choice(name: 'ACTION', choices: ['build', 'test', 'deploy'], description: 'Build action')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/varaprasad070693/sonarqube.git'
            }
        }

        stage('Print Parameters') {
            steps {
                echo "Running in environment: ${params.ENV}"
                echo "Action selected: ${params.ACTION}"
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: '3c0e6ed1-daf1-49fe-9425-e7f182d6c6af', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQube') {
                        script {
                            // Get Sonar Scanner path
                            def scannerHome = tool 'sonar_scanner'  // this name must match the one in Global Tool Config
                            sh """
                              ${scannerHome}/bin/sonar-scanner \
                                -Dsonar.projectKey=myproject \
                                -Dsonar.sources=. \
                                -Dsonar.host.url=http://13.202.137.9:9000 \
                                -Dsonar.login=$SONAR_TOKEN
                            """
                        }
                    }
                }
            }
        }
    }
}
