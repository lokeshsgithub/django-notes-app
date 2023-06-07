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
                   sh "docker image build -t $JOB_NAME:v1.$BUILD_ID ."
                   sh "docker image tag $JOB_NAME:v1.$BUILD_ID lokeshsdockerhub/$JOB_NAME:v1.$BUILD_ID"
                   sh "docker image tag $JOB_NAME:v1.$BUILD_ID lokeshsdockerhub/$JOB_NAME:latest"

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
                
                   sh "docker login -u lokeshsdockerhub -p ${dockerhub_pwd}"
                   sh "docker push lokeshsdockerhub/$JOB_NAME:v1.$BUILD_ID"
                   sh "docker push lokeshsdockerhub/$JOB_NAME:latest"
                    
                    }

                }
            }
        }

        stage('Update the image tag in YAML file'){

            steps{

                sh "sed -i 's/version/${BUILD_ID}/g' deployment.yaml"
            }
        }

        stage('Deploy the app into k8s cluster'){

            steps{

                script{

                sh """
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml

                """

                }

            }
        }
     
        
    }
}
