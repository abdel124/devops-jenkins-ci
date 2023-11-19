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
        NEXUSIP = "172.31.11.63"
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
    }

}
