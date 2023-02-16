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
    stage('zip lambda 1 and lambda2'){
      steps{
        echo 'zip lambda 1 and lambda2'
        script{
          sh 'zip lambda_function.zip pymysql PyMySQL-1.0.2.dist-info lambda_function.py'
          sh 'zip lambda_function2.zip pymysql PyMySQL-1.0.2.dist-info lambda_function2.py'
        }
      }

    }
    stage('upload lambda 1 and lambda 2'){
      steps{
        script{
          echo 'upload'
          sh 'aws lambda update-function-code --function-name eric_lambda --zip-file fileb://lambda_function.zip'
          sh 'aws lambda update-function-code --function-name eric_lambda2 --zip-file fileb://lambda_function2.zip'
          def status1 =""
          def status2=""
          def check=true
        }
      }
    }//LastUpdateStatus Successful

    stage('check status lambda'){
      steps{
        script{
          while(check){
            sh 'aws lambda get-function --function-name eric_lambda| tee lambda1.txt'
            sh 'aws lambda get-function --function-name eric_lambda2| tee lambda2.txt'
            status1=sh(script:'cat lambda1.txt|jq -r ".Configuration.LastUpdateStatus"',returnStdout:true ).toString().trim()
            status2=sh(script:'cat lambda2.txt|jq -r ".Configuration.LastUpdateStatus"',returnStdout:true ).toString().trim()
            if(status1.equals("Successful") && status2.equals("Successful")){
              check=false
            }
            else{
              echo status1
              echo status2
            }
          }
        }

      }



    }

  }
}