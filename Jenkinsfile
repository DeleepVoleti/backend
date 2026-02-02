pipeline {
    agent {
        label 'Agent-1'
    }

    options {
        timeout(time: 10, unit: 'MINUTES')
        ansiColor('xterm')
    }

    environment {
        APP_VERSION = ''   // ✅ environment variables are UPPERCASE
    }

    stages {

        stage('read app version') {
            steps {
                script {
                    def json = readJSON file: 'package.json'
                    env.APP_VERSION = json.version   // ✅ correct way
                }
            }
        }

        stage('install dependencies') {
            steps {
                sh """
                  npm install
                  ls -ltr
                  echo ${APP_VERSION}
                """
            }
        }

        stage('zipping the files') {
            steps {
                sh """
                  zip -r -q backend-${APP_VERSION}.zip * \
                    -x Jenkinsfile \
                    -x backend-${APP_VERSION}.zip
                """
            }
        }
    }

    post {
        always {
            deleteDir()
        }
    }
}
