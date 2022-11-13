pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5'))
        timestamps()
        timeout(time: 2, unit: 'HOURS')
        skipDefaultCheckout()
    }

    tools {
        
        maven 'maven3.6'
      
    }

    environment {

        GIT_CREDENTIALS_ID = 'anji_git_hub_acct'
        SOURCE_GIT_REPO = 'https://github.com/Techsoultions/Sample_Project.git'
        BUILD_NUM = "${BUILD_ID}"
    
    }

    stages{

        stage('Checkout code for SCM')
        {
          steps
          {
            script{
               echo " Code Check out from github......... "  
               git branch: 'main', credentialsId: "${GIT_CREDENTIALS_ID}", url: "${SOURCE_GIT_REPO}"   
            }
          }
        }

        stage('Buildig,testing and generating artifact(WAR or JAR)')
        {
          steps
          {
            script{
                echo " Buildig,testing and generating artifact!!!!!!!!!!!!!! "
                sh 'mvn clean install'
           }
          }
        }

        stage('Building Docker image')
        {
            steps{
                script{

                   sh " ansible-playbook Ansible_play_books/docker_image_creation_in_asnisblenode.yml "
                   //ansiblePlaybook become: true, installation: 'ansible', inventory: 'Ansible_play_books/invent.iti', playbook: 'Ansible_play_books/docker_image_creation_in_asnisblenode.yml'
                }
            }

        }
        
    }

    post {
    always {
      deleteDir() /* clean up our workspace */
    }

    success {
      script {
        echo " Removing the local docker images "
        sh ' docker rmi -f $(docker images -q) '
      }
    }

}
}
