qualitygate = 'Not runned yet'

pipeline {
    agent {
        label 'master'
    } 
    environment {
        envName = 'dev'
    }
    stages {
        stage ("Git pull"){
            steps{
                echo 'checkout the Code'
                checkout scm                
            }
        }
        stage("Docker clean"){
            steps{  
                sh 'pip install --user aws-sam-cli'
                sh 'USER_BASE_PATH=$(python -m site --user-base)'
                sh 'export PATH=$PATH:$USER_BASE_PATH/bin'  
            }
        }
        stage("Docker build"){
            steps{
                sh "set -e; for i in $(find | grep 'tests/unit$' | cut -d'/' -f1-3); do cd \"$i\"; npm install; npm run test; cd ../..; done)"
                sh "(set -e; for i in $(find | grep 'events/' | cut -d'/' -f1-2); do cd \"$i\"; sam build; sam package --s3-bucket awscoursepetosbucket --s3-prefix builds; cd ..; done)"
            }
        }      
    }
    post {
    }
}