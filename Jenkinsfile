pipeline {
    agent any

    stages {
        stage ('Checkout') {
          steps {
            git branch: 'master',
            credentialsId: 'gitcreds',
            url: 'https://github.com/mdangol/lambdadeploy.git'
          }
        }
        stage('Build') {
            steps {
                withAWS(credentials: 'awscreds', region: 'us-east-1'){
                    sh 'sam package --template-file template.yaml --output-template-file package.yaml --s3-bucket k8-app-demo --region us-east-1'
                }
            }
        }
       stage('Deploy Dev') {
        steps {
          withAWS(credentials: 'awscreds', region:'us-east-1') {
              sh 'sam deploy --template-file ./package.yaml --stack-name testsam --region us-east-1 --capabilities CAPABILITY_IAM'
          }
    }
}
    }
}
