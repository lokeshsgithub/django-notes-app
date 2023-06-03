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
                   sh "docker image build -t $JOB_NAME:v1.$JOB_BUILD ."
                   sh "docker image tag $JOB_NAME:v1.$JOB_BUILD lokeshsdockerhub/$JOB_NAME:v1.$JOB_BUILD"
                   sh "docker image tag $JOB_NAME:v1.$JOB_BUILD lokeshsdockerhub/$JOB_NAME:latest"
                }
            }
        }

        stage('Scan the docker image'){

            steps{
                
                script{

                    sh """
                       trivy image lokeshsdockerhub/$JOB_NAME:latest > scan.txt
                       cat scan.txt

                    """
                }
            }
        }

        stage('Push image into dockerhub'){

            steps{

                script{
                    
                    withCredentials([string(credentialsId: 'docker_auth', variable: 'dockerhub_pwd')]) {
                    
                    sh """
                    docker login -u lokeshsdockerhub -p ${dockerhub_pwd}
                    docker push lokeshsdockerhub/$JOB_NAME:v1.$JOB_BUILD
                    docker push lokeshsdockerhub/$JOB_NAME:latest
                    """
                    }

                }
            }
        }


        
    }
}

        

 