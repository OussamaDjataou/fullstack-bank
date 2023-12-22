@Library('library') _
pipeline {
    agent any
    
    tools{
        jdk 'jdk17'
        nodejs 'node16'
        
    }
    
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                GitCheckout()
            }
        }
        
        stage('OWASP FS SCAN') {
            steps {
                dependencyCheck()
            }
        }
        
        
        
        stage('SONARQUBE ANALYSIS') {
            steps {
                SonarAnalysis()
                
            }
        }
        
        
         stage('Install Dependencies') {
            steps {
                NpmDependenciesInstall()
            }
        }
        
        stage('Backend') {
            steps {
                BackendContainerBuild()
            }
        }
        
        stage('Backend scan') {
            steps {
                BackendImageScan()
                }
        }
        
        
        stage('frontend') {
            steps {
                FrontendContainerBuild()
            }
        }


        stage('Front scan') {
            steps {
                FrontendImageScan()
            }
        }
        
        stage('Deploy to Conatiner') {
            steps {
                Deployment()
            }
        }
    }
}
