pipeline {
    agent any
    //tools {
    //    maven = "maven"
    //    //gradle = "gradle"
    //    jdk    = "jdk1.8"
    //}
    parameters {
        choice(name: "Version", choices: ["1.1.0", "1.2.0", "1.3.0"], description: "")
        booleanParam(name: "executeTests", defaultValue: true, description: "")

    }
    environment {
        // NEW_VERSIOM = "1.3.0"
        // SERVER_CREDENTIALS = credentials("f3d6aeac-a9da-430d-bc3d-540684eaf2c8")
        // branch = 'master'
        scmUrl = "ssh://git@github.com/jefff-guu/Jenkins_Lab.git"
        serverPort = "8080"
        devServer = "dev-myproject.mycompany.com"
        qaServer = "staging-myproject.mycompany.com"
        prodServer = "production-myproject.mycompany.com"
    }

    triggers {
        pollSCM("H/5 * * * 1-5")
    }
    
    stages {
        stage("Checkout") {
            steps {
                git branch: branch, credentialsId: 'GitCredentials', url: scmUrl
            }
        }
        stage("Build") {
            // Groovy expression:
            // when {
            //     expression {
            //         BRANCH_NAME == "dev"
            //     }
            // }

            when {
                branch "dev"
            }
            steps {
                echo "building the application"
                echo "building version ${NEW_VERSIOM}"
                // sh "mvn clean install"
            }
        }
        stage("Test") {
            when {
                expression {
                    // BRANCH_NAME == "dev" || BRANCH_NAME == "master"
                    params.executeTests == true
                }
            }
            steps {
                echo "testing the applicaiton"
            }
        }

        stage("Deploy to Dev") {
            when {
                branch "dev"
            }
            steps {
                echo "deploying the application"
                echo "deploying version ${params.VERSION}"
            }
        }
        stage("Deploy to QA") {
            when {
                branch "QA"
            }
            steps {
                echo "deploying the application"
                echo "deploying version ${params.VERSION}"
            }
        }
        stage("Approve to Prod?") {
            input {
                message "Will you approve to deploy to Prod?"
                ok "Yes, approved"
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps {

                echo "Continue to deploy"
            }
        }

        stage("Deploy to Prod") {
            when {
                branch "Prod"
            }
            steps {
                echo "deploying the application"
                echo "deploying version ${params.VERSION}"
            }
        }
    }

    post{
        //cleanup workspace
        always{
            echo "Clean up workspace......"
            deleteDir()
        }
        failure{
            failure {
                mail to: 'jeff.gu@me.com', subject: 'Pipeline failed', body: "${env.BUILD_URL}"
            }
        }
    }
}
