pipeline {
    agent any
    tools {
        maven "MAVEN"
        jdk "JDK17"
    }
    
    environment {
        SNAP_REPO = 'vprofile-snapshot'
		NEXUS_USER = 'admin'
		NEXUS_PASS = 'admin'
		RELEASE_REPO = 'vprofile-releases'
		CENTRAL_REPO = 'vprofile-maven-central'
		NEXUSIP = '172.31.14.220'
		NEXUSPORT = '8081'
		NEXUS_GRP_REPO = 'vprofile-maven-group'
        NEXUS_LOGIN = 'nexus_ID'
    }

    stages {
        stage('Build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
        }
    }
}