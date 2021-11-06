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
        withSonarQubeEnv(credentialsId: 'f536102f-e5d1-4741-b46a-df2b8d4b3f9a') {
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
        nexusArtifactUploader artifacts: [[artifactId: '$BUILD_TIMESTAMP', classifier: '', file: 'target/vprofile-v1.war', type: 'war']], credentialsId: '5e9c9e1e-e0c9-462f-b20a-1eed731c752c', groupId: 'Dev', nexusUrl: '172.31.8.189:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'vprofile-repo', version: '$BUILD_ID'
    }
}
