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
        
    }
}

        

 