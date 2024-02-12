pipeline {
    agent {
        node {
            label 'agent-1'
        }      
    }
    environment { 
            packageVersion = ''
            nexusurl = '172.31.11.253:8081'
    }
   
    options {
                timeout(time: 1, unit: 'HOURS')
                disableConcurrentBuilds() 
            }
    parameters {
        
        booleanParam(name: 'deploy', defaultValue: false, description: 'Toggle this value')
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
         stage('unit test') {
            steps {
                sh """
                   echo "unit test cases will run here"
                """
            }
        } 
         stage('sonarscan') {
            steps {
                sh """
                   sonar-scanner
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
        
        stage('publish Artifacts') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexusurl}",
                    groupId: 'com.roboshop',
                    version: "${packageVersion}",
                    repository: 'catalogue',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                         [artifactId: 'catalogue',
                         classifier: '',
                         file: 'catalogue.zip',
                         type: 'zip']
                    ]
                )  
     
                
            }
        }
        stage('Deploy') {
            steps {
                when {
                    expression{
                        params.deploy == 'true'
                    }
                }
                script {
                    def params = [
                        string(name: 'version', value: "${packageVersion}"),
                        string(name: 'environment', value: "dev")
                    ]

                        build job: "catalogue-deloy", wait: true ,parameters: params

                }
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


     
