pipeline {

    agent any

    stages('Checkout code'){

        steps{

           git branch: 'main',
           credentialsId: 'github_auth',
           pwdurl: ' https://github.com/lokeshsgithub/django-notes-app.git'
        }
    }

}