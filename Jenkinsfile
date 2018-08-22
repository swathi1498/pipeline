pipeline {
	    agent any
	    stages{
	        stage('Build'){
	            steps {
	                sh 'mvn clean package'
	            }
	            post {
	                success {
	                    echo 'Now Archiving...'
	                    archiveArtifacts artifacts: '**/target/*.war'
	                }
	            }
	        }
	        stage ('Deploy to Dev'){
	            steps {
	                build job: 'Maven_Project _3_Dev_Tomcat'
	            }
	        }
	
	        stage ('Deploy to Production'){
	            steps{
	                timeout(time:5, unit:'DAYS'){
	                    input message:'Approve PRODUCTION Deployment?'
	                }
	
	                build job: 'Maven_Project_4_Prod_Tomcat'
	            }
	            post {
	                success {
	                    echo 'Code deployed to Production.'
	                }
	
	                failure {
	                    echo ' Deployment failed.'
	                }
	            }
	        }
	
	
	    }
	}
