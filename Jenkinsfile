pipeline {
    agent any

   environment {
        GIT_URL = 'https://github.com/PramodTestHub/APIProject'
        GIT_BRANCH = 'main'
        JAVA_HOME = "C:/Program Files/Java/jdk-21"
        MAVEN_HOME = "C:/Users/admin/Downloads/apache-maven-3.9.11-bin/apache-maven-3.9.11"
        PATH = "${JAVA_HOME}/bin;${MAVEN_HOME}/bin;${env.PATH}"
    }
    triggers {
        cron('H 8,20 * * *')   
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: "${env.GIT_BRANCH}", url: "${env.GIT_URL}"
            }
        }

        stage('Build & Run Tests') {
            steps {
                echo 'Running Rest assured-Cucumber Tests...'
                bat "mvn -version"
                bat "mvn clean test -Dsurefire.useFile=false"
            }
        }

        stage('Generate Allure Report') {
            steps {
                allure([
                    includeProperties: false,
                    jdk: '',
                    commandline: 'allure',  
                    results: [[path: 'target']]
                ])
            }
        }
    }

    post {
        always {
            echo 'Archiving Test Results...'
            archiveArtifacts artifacts: 'target/**', fingerprint: true
        }
        failure {
            echo 'Build Failed!'
        }
    }
}
