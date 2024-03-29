def gv
pipeline{
    agent any
    environment{
        NEW_VERSION = '1.1'
        BRANCH_NAME = "${scm.branches[0].name}"
    }
    stages{
        stage('confirm build'){
            when {
                expression {
                    GIT_BRANCH =~ 'main'
                }
            }
            input {
                message 'Do you want to continue with the build?'
                ok "Done"
                parameters {
                    choice(name: 'confirmBuild', choices: ['yes','no'], description: '')
                }
            }
            steps {
                script {
                        if (confirmBuild == 'no') {
                            error('Build aborted by user')
                    }
                }
            }
        }
        stage("init"){
            steps{
                script{
                    gv = load "jenkins.groovy"
                }
            }
        }
        stage("build"){
            when{
                expression {
                    BRANCH_NAME =~ "main"
                }
            }
            steps{
                echo "Start build"
                echo "========stage build - steps ========"
                echo "BRANCH_NAME is ${BRANCH_NAME}"
                echo "GIT_BRANCH is ${GIT_BRANCH}"
                script{
                    gv.buildapp()
                }
                echo "End of build"
            }
        }
        stage("test"){
            steps{
                echo "Start test"
                echo "========stage test - steps ========"
                echo "End of test"
            }
        }
        stage("deploy"){
            steps{
                echo "Start deploy"
                echo "========stage deploy - steps ========"
                echo "The new version is ${NEW_VERSION}"
                echo "end of deploy"
            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}