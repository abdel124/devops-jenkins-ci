pipeline {
    
	agent any
    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_USER = "admin"
        NEXUS_PASS = "admin123"
        SNAP_REPO = "vprofile-snapshot"
        RELEASE_REPO = "vprofile-release"
        CENTRAL_REPO = "vpro-maven-central"
        NEXUS_GRP_REPO = "vpro-maven-group"
        NEXUS_LOGIN = "nexuslogin"
        NEXUSIP = "172.31.11.17"
        NEXUSPORT= "8081"

    }
    stages{
        stage('Build'){
            steps{
                sh "mvn -s settings.xml -DskipTests install"
            }
            post{
                success {
                    echo "Now Archiving"
                    archiveArtifacts artifacts: '**/*.war/'
                }
            }
        }
        stage('Test'){
            
            steps{
                sh 'mvn test'
            }
        }
        stage('Checkstyle Analysis'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }

        }
        stage('upload artifcat to nexus'){
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${NEXUSIP}:${NEXUSPORT}",
                    groupId: 'QA',
                    version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                    repository: "${RELEASE_REPO}",
                    credentialsId: "${NEXUS_LOGIN}",
                    artifacts: [
                        [artifactId: 'vproapp',
                        classifier: '',
                        file: 'target/vprofile-v2.war',
                        type: 'war']
                    ]
                )
            }

        }
    }

}
