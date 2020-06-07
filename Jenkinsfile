pipeline {
    agent any
    //tools {
    //    maven = "Maven"
    //    gradle = "Gradle"
    //    jdk    = "JDK"
    //}
    parameters {
        choice(name: "Version", choices: ["1.1.0", "1.2.0", "1.3.0"], description: "")
        booleanParam(name: "executeTests", defaultValue: true, description: "")
    }
    environment {
        NEW_VERSIOM = "1.3.0"
        SERVER_CREDENTIALS = credentials("f3d6aeac-a9da-430d-bc3d-540684eaf2c8")
    }
    stages {
        stage('build') {
            when {
                expression {
                    BRANCH_NAME == "dev"
                }
            }
            steps {
                echo "building the application"
                echo "building version ${NEW_VERSIOM}"
                // sh "mvn clean install"
            }
        }
        stage('test') {
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
        stage('deploy') {
            steps {
                echo "deploying the application"
                echo "deploying version ${params.VERSION}"
            }
        }
    }

    post {
        always {
            echo "always check whatever it fails or succeeds"
        }
        success {
            echo "run only on success"
        }
        failure {
            echo "run only on failure"
        }
    }
}
