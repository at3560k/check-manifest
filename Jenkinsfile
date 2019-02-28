pipeline {
    agent none

    stages {
        stage('Allocate') {
            agent any
            steps {
                deleteDir()
                checkout scm
                stash 'workspace'

            }
        }
        stage('Run Tests') {
            parallel {
                stage('Test on py35') {
                    agent {
                        label "py35"
                    }

                    steps {
                        echo 'Testing'
                        unstash 'workspace'
                        sh 'tox -e py35'
                        sh 'tox -e flake8'
                    }
                }
                stage('Test on py34') {
                    agent {
                        label "py34"
                    }

                    steps {
                        echo 'Testing'
                        unstash 'workspace'
                        sh 'tox -e py34'
                    }
                }

            }
        }
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
