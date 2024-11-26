pipeline {
    agent any
    
    tools {
        nodejs 'TINnode-devops'
    }

    stages {
        
         stage('Fetching Source') {
            steps {
                echo 'Fetching source from personal repository...'
                git branch: 'main', credentialsId: 'Dmitrii-Iurev', url: 'git@github.com:Dmitrii-Iurev/calculator-app-finished.git'
            }
         }

        stage('install dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
            }
        }

        stage('unittest') {
            steps {
                echo 'Running unit tests...'
                sh 'npm test -- --reporters=default --reporters=jest-junit'
                junit 'junit.xml'
            }
        }

        stage('create bundle') {
            steps {
                script {
                    echo 'Creating bundle...'
                    sh '''
                    mkdir -p bundle
                    cp -r app package.json package-lock.json bundle/
                    zip -r bundle.zip bundle
                    '''
                }
            }
        }
    }

    post {
        failure {
            sh '''
            echo "pipeline poging faalt op $(date)" >> /home/jenkins/jenkinserrorlog
            '''
        }
        
        success {
            archiveArtifacts artifacts: './bundle.zip'
        }
        
        }

    }

