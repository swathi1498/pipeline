pipeline {
    agent any
    stages{
        stage('Init'){
            steps {
                echo "Testing..."
                emailext body: '''Hi Team

                Thanks''', subject: 'Failure - SonarQube Analysis', to: 'vevadevops@gmail.com'
            }
        }

        stage('Build'){
            steps {
                echo "Building..."
            }
        }

        stage('Deploy'){
            steps {
                echo "Code Deployed"
            }
         }
     }
}
