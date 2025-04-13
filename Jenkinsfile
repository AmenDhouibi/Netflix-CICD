pipeline {
    agent any
    tools {
        jdk 'jdk'         // Must match JDK name in Global Tool Configuration
        nodejs 'node'      // Must match NodeJS name in Global Tool Configuration
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'  // Must match the Sonar Scanner name in Global Tool Configuration
    }
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/Aj7Ay/Netflix-clone.git'
            }
        }
       stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') { 
                    sh """
                        \$SCANNER_HOME/bin/sonar-scanner \\
                          -Dsonar.projectName=Netflix \\
                          -Dsonar.projectKey=Netflix
                    """
                }
            }
        }
        stage('Quality Gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar_token'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        
        stage('TRIVY FS SCAN') {
            steps {
                // Scans the current directory and writes results to trivyfs.txt
                sh "trivy fs . > trivyfs.txt"
            }
        }
