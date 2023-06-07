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

        stage('Update the image tag in YAML'){

            steps{

                sh """
                cd notesapp
                sed -i 's/version/${BUILD_ID}/g' values.yaml > scan.txt
                cat scan.txt

                git config --global user.email "lokeshreddy05690@gmail.com"
                git config --global user.name "lokeshsgithub"
                git add .
                git commit -m "update the image tag in YAML file"
                git push origin main
                """
            }
        }


        stage('Deploy the app into helm '){

            steps{

                script{

                sh """
                helm upgrade --install noteapp notesapp --dry-run > scan.txt
                cat scan.txt
                helm upgrade --install noteapp notesapp
                """

                }

            }
        }
     
        
    }
}
