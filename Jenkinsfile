pipeline {

    agent any 

    environment {

                def shortCommit = sh(returnStdout: true, script: "git log -n 1 --pretty=format:'%h'").trim()
                def author = sh(returnStdout: true, script: "git show -s --pretty=%an").trim()
                DOCKERHUB_CREDENTIALS = credentials('jenkins-hub')
            }



             stages {
    stage('Checkout') {
      steps {
        
        git(  url: 'https://github.com/lechiffresene/Mailu.git', branch: 'main' ) 
        
      }
    }



            stage('Build image') {

                steps {
                        sh "docker build -t cdelambert/Mailu-odoo:${shortCommit}  ."


                }
            }

            stage('Login') {
                steps {
                         sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                 
                }
          }


            stage('Push image') {
                
                steps {
                        sh "docker push cdelambert/Mailu-odoo:${shortCommit} "


                }
            }

            stage('Clean image') {
                
                steps {
                        sh "docker rmi cdelambert/Mailu-odoo:${shortCommit} "

                }
            }
           
    }

}