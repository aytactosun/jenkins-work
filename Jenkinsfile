pipeline {
    agent any
    stages {
        stage{'Checkout'}{
            steps {
                checkout scm
            }
        }
        stage{'Build'}{
            steps {
                echo 'Building the project...'
                sh 'gcc jenkisn-hello.c -o jenkins-hello.out'
            }
        }
        stage{'Run'}{
            steps {
                echo 'Running the project...'
                sh './jenkins-hello.out'
            }
        }
    }
}