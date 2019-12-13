pipeline {
	    agent any
	
	    parameters {
	         string(name: 'tomcat_dev', defaultValue: '172.31.40.243', description: 'Staging Server')
	         string(name: 'tomcat_prod', defaultValue: '172.31.45.164', description: 'Production Server')
	    }
	
	    triggers {
	         pollSCM('* * * * *')
	     }
	
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
	
	        stage ('Deployments'){
	            parallel{
	                stage ('Deploy to QA'){
	                    steps {
	                        sh "scp -p -r /var/lib/jenkins/workspace/maven_project_pipeline_as_code/target/vprofile-v1.war jenkins@${params.tomcat_dev}:/usr/local/apache-tomcat-8.5.49/webapps"
	                    }
	                }
	
	                stage ("Deploy to Production"){
	                    steps {
	                        sh "scp -p -r /var/lib/jenkins/workspace/Jenkins_Pipeline_As_Code/target/vprofile-v1.war jenkins@${params.tomcat_prod}:/usr/local/apache-tomcat-8.5.49/webapps"
	                    }
	                }
	            }
	        }
	    }
	}
