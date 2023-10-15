
pipeline {
    agent any
    tools {
	    maven 'maven-jenkins'
    }
    
    stages {
        stage('MAVEN BUILD'){
            steps{
                sh 'mvn clean package -Dmaven.test.skip=true'
                sh 'mv target/*.jar target/mavencommit02.jar'
            }
        }
        stage('TEST'){
            steps{
                sh 'mvn test'
            }
        }
        stage('DEPLOY'){
            steps {
                sshagent(['jenkins-agent-creds']) {
                    sh '''
                
                    scp -o StrictHostKeyChecking=no target/mavencommit02.jar ec2-user@34.209.47.66:/opt/tomcat9/webapps
                    ssh ec2-user@34.209.47.66 /opt/tomcat9/bin/shutdown.sh
                    ssh ec2-user@34.209.47.66 /opt/tomcat9/bin/startup.sh
                
                    '''
                }
            }
        }
    }
    
    post {
        success { 
            echo "This pipeline is successfull!"
            }
    unsuccessful {
            echo "ISSUE!!!"
            }
    }
}