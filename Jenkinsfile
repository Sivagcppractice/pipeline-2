pipeline {
    agent any
    tools {
        maven 'mvn-3.9.6'
    }
    stages {
        stage('pipeline job') {
            steps {
                echo 'welcome second pipeline'
            }
        }
        stage('checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/Release_1.1']], extensions: [], userRemoteConfigs: [[credentialsId: 'Git', url: 'https://github.com/Sivagcppractice/mvn-pra2.git']])
            }
        }
        stage('Build') {
            steps {
                sh 'mvn package -f pom.xml'
            }
        }
        stage('release-deploy') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'Tomcat9', path: '', url: 'http://34.42.162.57:8090')], contextPath: null, war: 'target/Amazon.war'
            }
        }
        stage('Approvals for release team') {
            steps {
                echo "Taking approval from Manager to release wars"
                timeout(time: 7, unit: 'DAYS') {
                    input message: 'Do you want to deploy wars in stage env?', submitter: 'gcppractice560'
                }
            }
        }
        stage('Nexus_repo') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'Amazon', classifier: '', file: 'target/Amazon.war', type: 'war']], credentialsId: 'Nexus', groupId: 'com.gcp', nexusUrl: '34.172.173.119:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'Amazon-proj', version: '1.0-SNAPSHOT'
            }
        }
    }
}

