pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo "📦 Checking out repository"
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "🔧 Compiling hello.c..."
                sh 'gcc hello.c -o hello.out'
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
