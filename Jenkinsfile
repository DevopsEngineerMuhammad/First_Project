pipeline {
    agent any

    tools {
        maven 'M2_HOME'
        jdk 'JAVA_HOME'
    }

    stages {
        stage('Clean the workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checking out the code from GITHUB') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/DevopsEngineerMuhammad/First_Project.git'
            }
        }

        stage('Performing UNIT Test') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('Compile the Code') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Upload to S3 Artifactory') {
            steps {
                sh 'aws s3 cp /var/lib/jenkins/workspace/Pipeline_Script/webapp/target/webapp.war s3://cicdbuckethiba/Snapshot_Artifacts/CICDProject.war'
            }
        }

        stage('Deploy to Tomcat QA Server') {
            steps {
                sshagent(['tomcat']) {
                  sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/Pipeline_Script/webapp/target/webapp.war ec2-user@13.196.196.110:/opt/tomcat/tomcat/webapps/"
                }
            }
        }

    }
}
