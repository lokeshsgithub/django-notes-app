pipeline {

    agent any

    stages {

        stage('Checkout code'){
         
           steps{
              
              git branch: 'main',
              credentialsId: 'github_auth',
              url: ' https://github.com/lokeshsgithub/django-notes-app.git'
            }

        }

        stage('Build the docker image'){

            steps{
                
                script{

                    sh """
                    docker build -t $JOB_NAME:v1.JOB_BUILD .
                    docker image tag $JOB_NAME:v1.JOB_BUILD lokeshsdockerhub/$JOB_NAME:v1.JOB_BUILD
                    docker image tag $JOB_NAME:v1.JOB_BUILD lokeshsdockerhub/$JOB_NAME:latest

                    """
                }
            }
        }
        
    }
}

        

 