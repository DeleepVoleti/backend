pipeline{
    agent {
        label 'Agent-1'
    }
    options{
        timeout(time:10, unit:'MINUTES')
        ansiColor('xterm')
    }
    parameters {
        choice(
            name: 'ACTION',
            choices: ['apply', 'destroy'],
            description: 'Select Terraform action'
        )
    }
    stages{
        stage("init"){
            steps{

                
                sh """
                   ls
                """
            }
        }
        stage("plan"){
            when{
                expression{params.ACTION == 'apply'}
            }
            steps{
                sh """
                   cd 01-vpc
                   terraform plan
                """
            }
        }
        
      post{
        always{
            deleteDir()
        }
    }  
    }
}