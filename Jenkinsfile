pipeline {
    agent any
    tools {
         maven "MAVEN3.9"
        jdk "JDK17"

    }
    
    environment {
        SNAP_REPO = 'vprofile-snapshot'
		NEXUS_USER = 'admin'
		NEXUS_PASS = 'admin123'
		RELEASE_REPO = 'vprofile-release'
		CENTRAL_REPO = 'vpro-maven-central'
		NEXUSIP = '172.31.5.4'
		NEXUSPORT = '8081'
		NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
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
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                                -Dsonar.projectKey=vprofile \
                                -Dsonar.projectName=vprofile \
                                -Dsonar.sources=src \
                                -Dsonar.host.url=http://18.170.57.130/sonar \
                                -Dsonar.login=$SONAR_TOKEN \
                                -Dsonar.java.binaries=target/classes \
                                -Dsonar.junit.reportsPath=target/surefire-reports \
                                -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml \
                                -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml
                        """
              }
            }
        }
    }
}