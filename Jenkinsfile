pipeline {
  agent any 
  environment {
    AWS_ACCESS_KEY_ID = credentials('awsAccessKeyId') 
    AWS_SECRET_ACCESS_KEY = credentials('awsSecretAccessKey') 
    AWS_DEFAULT_REGION = 'ap-northeast-2'}
  stages {
    stage('Clone') {
      steps {
        echo 'Clone'
        git credentialsId: 'token_for_github', url: "https://github.com/ericsong917/lambda_pipe"
      }
    }
    dir('/var/lib/jenkins/workspace/lambda') { 
      stage('zip lambda 1 and lambda2'){
        echo 'zip lambda 1 and lambda2'
        script{
          sh 'zip lambda_function.zip pymysql PyMySQL-1.0.2.dist-info lambda_function.py'
          sh 'zip lambda_function2.zip pymysql PyMySQL-1.0.2.dist-info lambda_function2.py'
        }
      }
    }

  }
}