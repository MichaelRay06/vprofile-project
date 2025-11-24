pipeline {
    agent any
    tools {
        maven "MAVEN"
        jdk "JDK21"

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
   stage('Sonar Analysis') {
            environment {
                scannerHome = tool "${SONARSCANNER}"
            }
            steps {
               withSonarQubeEnv("${SONARSERVER}") {
                   withCredentials([string(credentialsId: 'sonar_token', variable: 'SONAR_TOKEN')]) {

                   sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.host.url=http://35.179.160.133/sonar \
                   -Dsonar.login="[$SONAR_TOKEN]" \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml '''
                
              }
            }
        }
    }
}