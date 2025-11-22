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

        // SonarQube settings
        SONARSERVER = "sonarserver"
        SONARSCANNER = "sonarscanner"
        SONAR_TOKEN = "vprofile_ID"
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo "Now Archiving."
                    archiveArtifacts artifacts: "**/*.war"
                }
            }
        }

        stage('Test') {
            steps {
                sh 'mvn -s settings.xml test'
            }
        }

        stage('Checkstyle Analysis') {
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

                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                          -Dsonar.projectKey=vprofile \
                          -Dsonar.projectName=vprofile \
                          -Dsonar.projectVersion=1.0 \
                          -Dsonar.sources=src \
                          -Dsonar.host.url=http://35.179.103.53 \
                          -Dsonar.login= "${SONAR_TOKEN}" \
                          -Dsonar.java.binaries=target/classes \
                          -Dsonar.junit.reportsPath=target/surefire-reports \
                          -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                          -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml
                        """
                    }
                }
            }
        }
    }

