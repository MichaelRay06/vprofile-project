pipeline {
    agent any
    tools {
        maven "MAVEN"
        jdk "JDK17"

    }
    
    environment {
         SNAP_REPO          = 'vprofile-snapshot'
        RELEASE_REPO       = 'vprofile-releases'
        CENTRAL_REPO       = 'vprofile-maven-central'
        NEXUSIP            = '172.31.14.220'
        NEXUSPORT          = '8081'
        NEXUS_GRP_REPO     = 'vprofile-maven-group'
        SONARSERVER        = "sonarserver"     // Jenkins global Sonar server name
        SONARSCANNER       = "sonarscanner"    // Jenkins global SonarScanner tool name
    }

    stages {
        stage('Build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo "Now Archiving."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }

        stage('Test'){
            steps {
                sh 'mvn -s settings.xml test'
            }

        }

        stage('Checkstyle Analysis'){
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }
    }
}