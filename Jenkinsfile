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
