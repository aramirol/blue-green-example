// JENKINSFILE

// Defining AWS Credentials
def credentialsForTestWrapper(block) {
    withCredentials([
        [
            $class: 'AmazonWebServicesCredentialsBinding',
            credentialsId: "aws_test",
            accessKeyVariable: 'AWS_ACCESS_KEY_ID',
            //secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
        ],
        [
            $class: 'AmazonWebServicesCredentialsBinding',
            credentialsId: "aws_test",
            //accessKeyVariable: 'AWS_ACCESS_KEY_ID',
            secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
        ],        
    ])
    {
        block.call()
    } 
}

// Set working directory

// Init pipeline
pipeline{
    agent any

    options {
        ansiColor('xterm')
        lock resource: 'terraform_landing_network'
    }

    stages {
        stage("Get Colour") {
            steps {
                credentialsForTestWrapper {
                    sh """
                    kubectl get service app-service-green -o jsonpath='{.spec.selector.app}'
                    """
                }
            }
        }

        stage("Switch") {
            steps {
                credentialsForTestWrapper {
                    sh "kubectl apply -f ingress_bluegreen.yml"
                }
            }
        }
    }

    post {
        cleanup {
            cleanWs()
        }
    }
}