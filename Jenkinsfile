pipeline {
    agent {
        node {
            label 'agent-1'
        }      
    }
    environment { 
            packageVersion = ''
    }
   
    options {
                timeout(time: 1, unit: 'HOURS')
                disableConcurrentBuilds() 
            }
   
    // build
    stages {
        stage('get version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    packageVersion = packageJson.version
                    echo "application version: $packageVersion"
                }
            }
        }
        stage('install dependencies') {
            steps {
                sh """
                   npm install
                """
            }
        } 
    
        stage('build') {
            steps {
                sh """
                   ls -la
                   zip  -q -r catalogue.zip ./* -x ".git" -x "*.zip"
                   ls -ltr
                """
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                sh """
                    echo "running shell script"
                """
            }
        }
        
    }
    // post build
    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        failure { 
            echo 'runs when their is failure'
        }
        success { 
            echo 'runs when success!'
        }
    }
}


     
