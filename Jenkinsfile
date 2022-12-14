pipeline{
    agent any

    environment{
        globalVar= 'I came from global env.'
        AUTHOR_EMAIL= 'mudassir@parseclabs.ca'
    }
    parameters{
        string(name: 'AUTHOR_NAME', defaultValue: 'Mudassir', description: 'This is author name')
        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')
        choice(name: 'CHOICE', choices: ['one', 'two', 'three'], description: 'You have your own choice')
        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter your password')
    }

    stages{
        stage('Commit Details') {
            steps {
                script {
                    env.CHANGE_COMMIT_MSG = sh (script: 'git log -1 --pretty=%B', returnStdout: true).trim()
                    env.CHANGE_AUTHOR = sh (script: 'git log -1 --pretty=%cn', returnStdout: true).trim()
                    env.CHANGE_AUTHOR_EMAIL = sh (script: 'git log -1 --pretty=%ae', returnStdout: true).trim()
                    env.CHANGE_COMMITOR_EMAIL = sh (script: 'git log -1 --pretty=%ce', returnStdout: true).trim()
                    env.CHANGE_HASH = sh (script: 'git log -1 --pretty=%h', returnStdout: true).trim()
                    echo "Author ${env.CHANGE_AUTHOR}"
                    echo "Author EMAIL: ${env.CHANGE_AUTHOR_EMAIL}"
                    echo "Commitor EMAIL: ${env.CHANGE_COMMITOR_EMAIL}"
                    echo "Commit Message:  ${env.CHANGE_COMMIT_MSG}"
                    echo "Commit Hash:  ${env.CHANGE_HASH}"
                }
            }
        }
        stage('Welcome Test'){
            environment{
                localVar= 'I came from local env.'
                name= 'Mudassir'
            }
            steps{
                echo "1: Hi ${name}, ${globalVar}"
                echo "2: And, ${localVar}"
            }
        }
        stage("Checking the Parameters"){
            steps{
                echo "Cheking the defined parameters..."
                echo "Hi, my name is ${params.AUTHOR_NAME}"
                echo "Toggle: ${params.TOGGLE}"
                echo "Choice: ${params.CHOICE}"
                echo "Passcode: ${params.PASSWORD}"
            }
        }
        stage("Checking the Input"){
            input{
                message "Should we continue"
                ok "Yes, offcourse"
            } 
            steps{
                echo "Hi ${AUTHOR_NAME}, Nice to meet you. I have checked the inputs successfully."
            }
        }
        stage("Checking the When"){
            when{
                equals expected: 1, actual: 1
            }
            steps{
                echo "If you are seeing this, when block is executed successfully..."
            }
        }
        stage("Cheking the Shell"){
            steps{
                echo "Checking the shell commands..."
                sh 'printenv'
            }
        }
        stage("Release"){
            steps{
                echo "Product is released..."
            }
        }
    }

    post{
        always{
            echo "Running post build steps..."
        }
        success{
            echo "Job was successfull..."
            echo "Sending email for job success..."
            
            mail charset: 'UTF-8', from: '', mimeType: 'text/html',
            to: "${env.CHANGE_AUTHOR_EMAIL}",
            replyTo: '',
            cc: '',
            bcc: '',
            subject: "Build successfull for Commit ${env.CHANGE_HASH} -> ${env.JOB_NAME}",
            body: "<br> Dear ${env.CHANGE_AUTHOR},<br><br> The build ${env.BUILD_NUMBER} for Project: ${env.JOB_NAME} completed successfully!<br><br>" +
            "Please visit the following URL to verify:<br> URL: ${env.BUILD_URL}<br><br>Regards,<br>Jenkins CI";
        }
        failure{
            echo "Job was Failed..."
            echo "Sending email for job failed..."

            mail charset: 'UTF-8', from: '', mimeType: 'text/html',
            to: "${env.CHANGE_AUTHOR_EMAIL}",
            replyTo: '',
            cc: '',
            bcc: '',
            subject: "Build Failed for Commit ${env.CHANGE_HASH} -> ${env.JOB_NAME}",
            body: "<br> Dear ${env.CHANGE_AUTHOR},<br><br> The build ${env.BUILD_NUMBER} for Project: ${env.JOB_NAME} Failed!<br><br>" +
            "Please visit the following URL to verify:<br> URL: ${env.BUILD_URL}<br><br>Regards,<br>Jenkins CI";
        }
        unstable {  
            echo 'Job is unstable!'
            echo 'Sending email for unstable job!'
            mail charset: 'UTF-8', from: '', mimeType: 'text/html',
            to: "${env.CHANGE_AUTHOR_EMAIL}",
            cc: '',
            replyTo: '',
            bcc: '',
            subject: "Build is Unstable -> ${env.JOB_NAME}",
            body: "<br> Dear Team,<br><br> The build for Project: ${env.JOB_NAME} is unstable!<br><br>" +
            "Please visit the following URL to review:<br> URL: ${env.BUILD_URL}<br><br>Regards,<br>Jenkins CI";
        }  
        changed {  
            echo 'This will run only if the state of the Pipeline has changed'  
            echo 'For example, if the Pipeline was previously failing but is now successful'  
        }  
    }

}
