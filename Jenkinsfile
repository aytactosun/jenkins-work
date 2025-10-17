pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo "🔄 Triggering build process"
                echo "📦 Checking out repository"
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "🔧 Compiling Project"
                sh 'gcc jenkins-hello.c -o hello.out'
            }
        }

        stage('Run') {
            steps {
                echo "🚀 Running compiled program..."
                sh './hello.out'
            }
        }
    }
}
