node{
    stage("CheckOutCode")
    {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/sivakethineni/CI-CD-project.git']]])
    }
    
    stage("Build")
    {
        sh 'mvn install'
    }
    
    stage("SonarQubeReport")
    {
        withSonarQubeEnv(credentialsId: 'ea776d04-3b89-4148-a42d-8643697f902d') {
             sh 'mvn sonar:sonar'
      }
    }
    
    stage("SonarQualityGateCheck"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
              else {
                  print "Pipeline success: ${qg.status}"
              }
          }
      }
    
    stage("ArtifactUploader"){
        nexusArtifactUploader artifacts: [[artifactId: '$BUILD_TIMESTAMP', classifier: '', file: 'target/vprofile-v1.war', type: 'war']], credentialsId: '5e9c9e1e-e0c9-462f-b20a-1eed731c752c', groupId: 'Dev', nexusUrl: '172.31.2.189:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'vprofile-repo', version: '$BUILD_ID'
    }
    stage("ArtifactUploader"){
        deploy adapters: [tomcat8(credentialsId: '6a509123-b706-425e-8eac-c1cb9714c189', path: '', url: 'http://172.31.12.96:8080')], contextPath: null, war: '**/*.war'   
    }
}
