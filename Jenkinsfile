pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment { 
        packageVersion = ''
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
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                sh """
                    echo  "Here I wrote shell script"
                    #echo "$GREETING"
                    #sleep 10
                """
            }
        }
        stage('check params'){
            steps{
                sh """
                    // echo "Hello ${params.PERSON}"

                    // echo "Biography: ${params.BIOGRAPHY}"

                    // echo "Toggle: ${params.TOGGLE}"

                    // echo "Choice: ${params.CHOICE}"

                    // echo "Password: ${params.PASSWORD}"

                    echo " hai "
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
            echo 'this runs when pipeline is failed, used generally to send some alerts'
        }
        success{
            echo 'I will say Hello when pipeline is success'
        }
    }
}
