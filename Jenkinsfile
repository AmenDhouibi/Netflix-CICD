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
       stage('Docker Build & Push') {
            steps {
                script {
                    // "docker" must match your Docker tool name and "docker" credentials ID
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh "docker build --build-arg TMDB_V3_API_KEY=d1ceb7c2512ecee0f4f227d3cb831d2f -t netflix ."
                        sh "docker tag netflix amendhouibi22/netflix:latest"
                        sh "docker push amendhouibi22/netflix:latest"
                    }
                }
            }
        }
        stage('TRIVY') {
            steps {
                // Scans the pushed Docker image and writes results to trivyimage.txt
                sh "trivy image amendhouibi22/netflix:latest > trivyimage.txt"
            }
        }
        stage('Deploy to container'){
            steps{
                sh 'docker run -d --name netflix -p 8081:80 amendhouibi22/netflix:latest'
            }
        }
       stage('Deploy to kubernets'){
            steps{
                script{
                    dir('Kubernetes') {
                        withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                                sh 'kubectl apply -f deployment.yml'
                                sh 'kubectl apply -f service.yml'
                        }   
                    }
                }
            }
        }
    }
