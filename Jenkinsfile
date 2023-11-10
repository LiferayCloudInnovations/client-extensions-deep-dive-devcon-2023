pipeline {

    agent any

    triggers {
        githubPush()
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh './gradlew clean build --no-daemon'
                }
            }
        }
        stage('Scan') {
            steps {
                withSonarQubeEnv('sonarqube-local') {
                    script {
                        //def scannerHome = tool 'sonarqube-scanner-cli-5.0.1.3006-linux';
                        //sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=myproject -Dsonar.sources=\"${env.WORKSPACE}\" -Dsonar.java.binaries=\"${env.WORKSPACE}/client-extensions/ticket-spring-boot/build,${env.WORKSPACE}/client-extensions/ticket-cleanup-cron/build\""
                        sh './gradlew sonar --no-daemon'
                    }
                }
            }
        }
    }
}

