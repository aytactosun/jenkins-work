pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo " Checking out repository"
                checkout scm
            }
        }

        stage('Static Analysis') {
            steps {
                echo " Running static code analysis for entire repository..."
                script {
                    // check cppcheck availability
                    def hasCppcheck = sh(script: 'command -v cppcheck >/dev/null 2>&1', returnStatus: true)

                    if (hasCppcheck == 0) {
                        echo "cppcheck found. Running full scan..."
                        sh '''
                            mkdir -p reports
                            # run cppcheck recursively on all source files and generate XML report
                            sh 'cppcheck --enable=all --inconclusive --suppress=missingIncludeSystem --xml --xml-version=2 .'
                        '''
                        // publish report to Jenkins UI
                        recordIssues(tools: [cppCheck(pattern: 'reports/cppcheck.xml')])
                    } else {
                        echo " cppcheck not found. Please install it using: sudo apt install cppcheck -y"
                    }
                }
            }
        }

        stage('Build') {
            steps {
                echo "Compiling all C files..."
                sh '''
                    SRC_FILES=$(find . -type f -name "*.c")
                    if [ -n "$SRC_FILES" ]; then
                        gcc $SRC_FILES -o output.out
                    else
                        echo " No C source files found to compile."
                    fi
                '''
            }
        }

        stage('Run') {
            steps {
                echo " Running compiled binary (if exists)..."
                sh '''
                    if [ -f ./output.out ]; then
                        ./output.out
                    else
                        echo " No binary found to execute."
                    fi
                '''
            }
        }
    }

    post {
        always {
            echo "📊 Archiving analysis reports..."
            archiveArtifacts artifacts: 'reports/*.xml', onlyIfSuccessful: false
        }
        success {
            echo "✅ Build and analysis completed successfully!"
        }
        failure {
            echo "❌ Build or analysis failed. Check console and reports for details."
        }
    }
}
