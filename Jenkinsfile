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
                git branch: 'main', url: 'https://github.com/OussamaDjataou/fullstack-bank.git'
            }
        }
        
        stage('OWASP FS SCAN') {
            steps {
                dependencyCheck additionalArguments: '--scan ./app/backend --disableYarnAudit --disableNodeAudit', odcInstallation: 'DC'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        
        
        stage('SONARQUBE ANALYSIS') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh " $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Bank -Dsonar.projectKey=Bank "
                }
            }
        }
        
        
         stage('Install Dependencies') {
            steps {
                sh '''npm install
                    npm audit fix --force
                    '''
            }
        }
        
        stage('Backend') {
            steps {
                dir('/var/lib/jenkins/workspace/Pipeline/app/backend') {
                    sh '''docker build -t backend .
                    '''
                }
            }
        }
        stage('Backend scan') {
            steps {
                dir('/var/lib/jenkins/workspace/Pipeline/app/backend') {
                    sh '''trivy image backend > scan1.txt
                    cat scan1.txt
                    '''
                }
            }
        }
        
        stage('frontend') {
            steps {
                dir('/var/lib/jenkins/workspace/Pipeline/app/frontend') {
                    sh '''docker build -t frontend .
                    '''
                }
            }
        }


        stage('Front scan') {
            steps {
                dir('/var/lib/jenkins/workspace/Pipeline/app/frontend') {
                    sh '''trivy image frontend > scan2.txt
                    cat scan2.txt
                    '''
                }
            }
        }
        
        stage('Deploy to Conatiner') {
            steps {
                dir('/var/lib/jenkins/workspace/Pipeline/app/frontend') {
                    sh ''' docker login -u djataououssama -p Owaxadjdada12*
                    docker tag frontend djataououssama/testjenkins:1.0 
                      docker push djataououssama/testjenkins:1.0
                         '''
                    
                }
            }
        }
    }
}
