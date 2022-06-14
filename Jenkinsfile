pipeline {
    agent {
        label 'maven'
    }

    parameters {
        booleanParam(description: 'Limpar o workspace antes de rodar o build', name: 'CLEAN_BEFORE_BUILD')
    }

    stages {
        stage('Clean') {
            when {
                anyOf {
                    expression { params.CLEAN_BEFORE }
                    branch 'master'
                }
            }


            steps {
                mvn('clean')
            }
        }

        stage('Build') {
            steps {
                mvn('compile')
            }
        }

        stage('Test') {
            steps {
                mvn('verify')
                junit 'target/surefire-reports/*.xml'
            }
        }

        stage('Deploy') {
            when {
                branch 'master'
            }

            steps {
                script {
                    def artifactId = readPom('project.artifactId')
                    def version = readPom('project.version')

                    withCredentials([string(credentialsId: 'super-deploy-secret', variable: 'SUPER_CREDENTIALS')]) {
                        sh "./super-deploy.sh $artifactId $version"
                    }

                    sh "./deploy.sh $artifactId $version"
                    currentBuild.description = "Deployed $artifactId version $version"
                }
            }
        }

    }
}

def mvn(String args) {
    sh "mvn --no-transfer-progress -B $args"
}

def readPom(String property) {
    return sh(script: """mvn help:evaluate -Dexpression="${property}" -q -DforceStdout""", returnStdout: true)
}



