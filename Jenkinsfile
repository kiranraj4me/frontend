  pipeline {
    agent any
    environment{  
      scmUrl = "git@github.com:kiranraj4me/frontend.git"
      scmBranch = "${env.BRANCH_NAME}" 
      shortCommit = sh(returnStdout: true, script: "git log -n 1 --pretty=format:'%h'")
      commitId = sh(returnStdout: true, script: "git rev-parse --short HEAD")
        
          }
 parameters {
               choice(choices: ['dev','uat','qa','PROD'], name: 'ENV')
                }
      
stages {
        stage('Clear Workspace') {
            steps {
                cleanWs()
                         
            }
        }
        stage('Checkout') {
            steps {
                   dir('app') {
                 
                git branch: scmBranch, credentialsId: '2b6a05fe-7c25-458d-9e5e-60e0d97a61c8', url: scmUrl
               }
            }

        }
         stage('Build') {
         environment{
           branchname = "${env.BRANCH_NAME}"
         }
            steps {

                  echo 'Building...'
            }            
        }
         stage('Test') {
         environment{
           branchname = "${ENV}"
         }
            steps {

                  echo 'Testing..'
            }            
        }
        stage('Deploy') {
          environment{
           prefix = "${ENV}"
         }
            steps {
                echo 'Deploying app...'
                      
                      sh 'aws s3 sync app/ s3://$prefix.ust-hackathon.com/'
                     

            }
        }
        stage('Finishing') {
            steps {
                echo 'Supdating cdn...'
                      sh ''
                  
            }
        }
    }

  }
