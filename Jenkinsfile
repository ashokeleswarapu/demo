pipeline {
    agent any
    
    stages {
        stage('Preliminary Tests') {
            steps {
                parallel (
                    "Framework Unit Testing": {
                        sh './test/test-wrapper.sh "Unit" 5'
                    },
                    "Functional Tests": {
                        sh './test/test-wrapper.sh "Functional" 10'
                    }
                )
            }
        }
        stage('SAST') {
            steps {
                sh './test/test-wrapper.sh "SAST" 15'
            }
        }
        stage('Automated Integration Tests') {
            steps {
                sh './test/test-wrapper.sh "Integration" 10'
            }
        }
        stage('Performance and Load Tests') {
            steps {
                sh './test/test-wrapper.sh "Load tests" 15'
            }
        }
        stage('Build') {
            when {
                branch 'master'
            }
            steps {
                sh './build.sh $(git rev-parse HEAD)'
                archiveArtifacts artifacts: 'build/*.deb', excludes: null
            }
        }
    }
}
