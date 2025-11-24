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
        NEXUS_LOGIN        = 'nexus_ID'
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
     stage("UploadArtifact"){
            steps{
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

