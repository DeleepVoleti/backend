pipeline{
    agent {
        label 'Agent-1'
    }
    options{
        timeout(time:10, unit:'MINUTES')
        ansiColor('xterm')
    }
    environment{
        def appVersion = '' // i defined app version here

    }
     
    stages{
        stage("read app version"){
            steps{
               script{
                def JSON = readJSON file: 'package.json'
                env.appVersion = JSON.version

               }
            }
        }

        stage("install dependencies"){
            steps{

                sh """
                   npm install
                   ls -ltr
                   echo $appVersion
                """
            }
        }

        // stage("zipping the files "){
        //     steps{

        //         sh """
        //        zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
        //         """
        //     }
        // }


    }
    
      post{
        always{
            deleteDir()
        }
    }  
}