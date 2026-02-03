pipeline {
    agent {
        label 'Agent-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    // parameters{
    //     booleanParam(name: 'deploy', defaultValue: false, description: 'Toggle this value')
    // }

    def appVersion = ''

    environment{
         appVersion = '' //variable declaration
         nexusUrl = 'nexus.dilipswebsite.online:8081'
        // region = "us-east-1"
        // account_id = "315069654700"
    }
    stages {
        stage('read the version'){
            steps{
                script{
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "application version: $appVersion"
                }
            }
        }
        stage('Install Dependencies') {
            steps {
               sh """
                npm install
                ls -ltr
                echo "application version: $appVersion"
               """
            }
        }
        stage('Build'){
            steps{
                sh """
                zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                ls -ltr
                """
            }
        }
        // stage('Docker build'){
        //     steps{
        //         sh """
        //             aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${account_id}.dkr.ecr.${region}.amazonaws.com

        //             docker build -t ${account_id}.dkr.ecr.${region}.amazonaws.com/expense-backend:${appVersion} .

        //             docker push ${account_id}.dkr.ecr.${region}.amazonaws.com/expense-backend:${appVersion}
        //         """
        //     }
        // }

        // stage('Deploy'){
        //     steps{
        //         sh """
        //             aws eks update-kubeconfig --region us-east-1 --name expense-dev
        //             cd helm
        //             sed -i 's/IMAGE_VERSION/${appVersion}/g' values.yaml
        //             helm upgrade backend .
        //         """
        //     }
       // }
        
        /* stage('Sonar Scan'){
            environment {
                scannerHome = tool 'sonar-6.0' //referring scanner CLI
            }
            steps {
                script {
                    withSonarQubeEnv('sonar-6.0') { //referring sonar server
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }

        stage("Quality Gate") {
            steps {
              timeout(time: 30, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
        } */

         stage('Nexus Artifact Upload'){
            steps{
                script{
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "${nexusUrl}",
                        groupId: 'com.expense',
                        version: "${appVersion}",
                        repository: "backend",
                        credentialsId: 'nexus-auth',
                        artifacts: [
                            [artifactId: "backend" ,
                            classifier: '',
                            file: "backend-" + "${appVersion}" + '.zip',
                            type: 'zip']
                        ]
                    )
                }
            }
        } 
         stage('Deploy'){
            steps{
                script{
                 def params = [
                    string(name:'appVersion', value: env.appVersion )
                 ]
                 build job :'backend-deploy', parameters: params ; wait:false
                }
            }
        } 
   }
    post { 
        always { 
            deleteDir()
        }
    }
}