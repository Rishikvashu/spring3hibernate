@Library('my-shared-lib') _

pipeline {

    agent any

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-8-openjdk-amd64'
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }

    stages {

        stage('Checkout') {

            steps {

                checkout scmGit(
                    branches: [[name: '*/master']],
                    userRemoteConfigs: [[url: 'https://github.com/opstree/spring3hibernate.git']]
                )
            }
        }

        stage('Security Scan') {

            steps {
                gitleaksScan()
            }
        }

        stage('Maven Lifecycle') {

            steps {
                mavenBuild()
            }
        }

        stage('Push Changes') {

            steps {
                gitPush()
            }
        }
    }

    post {

        always {

            archiveArtifacts(
                artifacts: 'gitleaks-report.json',
                fingerprint: true
            )
        }
    }
}
