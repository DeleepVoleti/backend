pipeline{
    agent {
        label 'Agent-1'
    }
    
    options{
        timeout(time:10, unit:'MINUTES')
        ansiColor('xterm')
    }
    
    environment{
        // Don't initialize with empty string, let it be set in the stage
        APP_VERSION = ''
    }
     
    stages{
        stage("read app version"){
            steps{
               script{
                    // Check if file exists first
                    if (fileExists('package.json')) {
                        def packageJson = readJSON file: 'package.json'
                        env.APP_VERSION = packageJson.version
                        echo "App version read: ${env.APP_VERSION}"
                    } else {
                        error("package.json not found in workspace!")
                    }
               }
            }
        }

        stage("install dependencies"){
            steps{
                script{
                    echo "Installing dependencies for version: ${env.APP_VERSION}"
                }
                sh """
                   npm install
                   ls -ltr
                   echo "App Version: $APP_VERSION"
                """
            }
        }

        stage('Build'){
            steps{
                sh """
                zip -q -r backend-$APP_VERSION.zip . -x Jenkinsfile -x backend-$APP_VERSION.zip
                ls -ltr
                """
            }
        }


    }
    
    post{
        always{
            deleteDir()
        }
    }  
}