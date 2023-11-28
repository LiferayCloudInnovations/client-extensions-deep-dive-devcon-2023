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
            environment {
                GITHUB_CREDS = credentials('jenkins-github-credentials')
                DEFAULT_BRANCH = "main"
            }
            steps {
                sh 'git fetch --no-tags --force --progress --prune -- https://${GITHUB_CREDS_USR}:${GITHUB_CREDS_PSW}@github.com/LiferayCloudInnovations/client-extensions-deep-dive-devcon-2023.git +refs/heads/${DEFAULT_BRANCH}:refs/remotes/origin/${DEFAULT_BRANCH}'

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

