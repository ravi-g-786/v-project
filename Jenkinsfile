node{
    stage('get code')
    {
        git credentialsId: '0525d1f9-9fff-4ec9-88da-5be951d36f13', url: ' https://github.com/ravi-g-786/v-project.git '
    }
    stage('build package')
    {
        def mavenHome = tool name:"maven-3.8.6", type:"maven"
        def mavenCMD = "${mavenHome}/bin/mvn"
        sh "${mavenCMD} clean package"
    }
    stage('code review')
    {
        withSonarQubeEnv('sonarqube-9.6.1')
       {
        def mavenHome = tool name:"maven-3.8.6", type:"maven"
        def mavenCMD = "${mavenHome}/bin/mvn"
        sh "${mavenCMD} sonar:sonar"
       } 
    }
    stage('uplode build artifact')
    {
        nexusArtifactUploader artifacts: [[artifactId: 'vprofile', classifier: '', file: 'target/vprofile-v2.war', type: 'war']], credentialsId: '3bb81bd5-5943-4a8b-a0a8-d1810e2633e8', groupId: 'com.visualpathit', nexusUrl: '3.92.201.67:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'vproject-release', version: 'v2'
    }
    stage('deploy')
    {
        sshagent(['982099b7-66e6-4e96-bd6b-93d4c119f1c2'])
        {
            sh 'scp -o StrictHostKeyChecking=no target/vprofile-v2.war ec2-user@100.26.104.192:/opt/apache-tomcat-9.0.78/webapps'
        }
    }
}
