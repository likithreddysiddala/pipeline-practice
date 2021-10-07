# pipeline-practice

pipeline{
    agent any
    stages{
        stage("scm checkout"){
            steps{
                git credentialsId: 'jenkins', url: 'https://github.com/likithreddysiddala/JavaHome-war.git'
            }
        }
        stage("maven build"){
            steps{
                sh'mvn clean package'
            }
        }
        stage("nexus"){
            steps{
               nexusArtifactUploader artifacts: [[artifactId: 'myweb', classifier: '', file: '/target/myweb-0.0.7-SNAPSHOT.war', type: '.war']], credentialsId: 'nexus', groupId: 'in.javahome', nexusUrl: '172.31.4.171', nexusVersion: 'nexus3', protocol: 'http', repository: 'http://54.206.124.239:8081/repository/likith-release/', version: '0.0.7-SNAPSHOT' 
            }
        }
    }
}
