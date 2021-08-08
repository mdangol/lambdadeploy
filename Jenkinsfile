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
                    sh '/usr/local/bin/sam package --template-file template.yaml --output-template-file package.yaml --s3-bucket k8-app-demo --region us-east-1'
                }
            }
        }
       stage('Deploy Dev') {
        steps {
          withAWS(credentials: 'awscreds', region:'us-east-1') {
              sh '/usr/local/bin/sam deploy --template-file ./package.yaml --stack-name testsam --region us-east-1 --capabilities CAPABILITY_IAM'
                }
            }
        }
        stage('Deploy Prod?'){
            steps {
                input("Deploy to Prod?")
                withAWS(credentials: 'awscredsprod', region: 'us-east-1'){
                    sh '/usr/local/bin/sam deploy --template-file ./package.yaml --stack-name prodsam --region us-east-1 --capabilities CAPABILITY_IAM'
                }
            }

        }
    }
}
