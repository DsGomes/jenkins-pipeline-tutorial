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
                expression { params.CLEAN_BEFORE_BUILD } 
            }

            steps {
                echo 'Stage Clean'
            }
        }

        stage('Build') {
            steps {
                echo 'Stage Compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Stage Test'
            }
        }

    }

}