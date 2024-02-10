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
    
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                sh """
                    echo "running shell script"
                    echo "$greeting"
                    # sleep 10
                """
            }
        }
        
    }
    // post build
    post { 
        always { 
            echo 'I will always say Hello again!'
        }
        failure { 
            echo 'runs when their is failure'
        }
        success { 
            echo 'runs when success!'
        }
    }
}


     
