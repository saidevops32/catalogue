pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment { 
        packageVersion = ''
        nexusURL = '172.31.13.142:8081'
    }
    options {
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()
    }
    // parameters {
    //     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

    //     text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

    //     booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

    //     choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

    //     password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    // }
    // build
    stages {
        stage('get the version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    packageVersion = packageJson.version
                    echo "appication version: $packageVersion"
                }
            }
        }
        stage('installing dependencies') {
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
                     zip -q -r catalogue.zip ./* -x ".git" -x "*.zip"
                     ls -ltr
                """
            }
        }
        stage('pushing artifact id') {
            steps {
                 nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "${nexusURL}",
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
                script {
                        def params = [
                            string(name: 'version', value: "$packageVersion")
                        ]
                        build job: "catalogue-deploy", wait: true, parameters: params
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
            echo 'this runs when pipeline is failed, used generally to send some alerts'
        }
        success{
            echo 'I will say Hello when pipeline is success'
        }
    }
}
