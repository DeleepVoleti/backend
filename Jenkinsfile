pipeline{
    agent {
        label 'Agent-1'
    }
    options{
        timeout(time:10, unit:'MINUTES')
        ansiColor('xterm')
    }
    environment{
        def app_version = '' // i defined app version here

    }
     
    stages{
        stage("read app version"){
            steps{
               script{
                def JSON = readJSON file: 'package.json'
                app_version = JSON.version

               }
            }
        }
    }

     stages{
        stage("install dependencies"){
            steps{

                sh """
                   npm install
                   ls -ltr
                   echo $app_version
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