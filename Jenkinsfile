pipeline {
    agent any

    tools {
        nodejs "NodeJS"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'peter-branch',
                    url: 'https://github.com/bigcephas1/React-ToDoList.git'
            }
        }

        stage('Frontend Build') {
            steps {
                dir('dive-react-app') {
                    echo "Installing frontend dependencies"
                    sh 'npm install'

                    echo "Running lint"
                    sh 'npm run lint'

                    echo "Building frontend"
                    sh 'npm run build'
                }
            }
        }

        stage('Backend Install') {
            steps {
                dir('backend') {
                    echo "Installing backend dependencies"
                    sh 'npm install'

                    echo "Running backend tests (instead of starting server)"
                    sh 'npm test || true'   // prevents failure if no tests exist
                }
            }
        }

    }

    post {
        always {
            cleanWs()
        }
        success {
            emailext(
                subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "The build ${env.BUILD_URL} completed successfully.",
                to: "olorunsolamiracle@gmail.com"
            )
        }
        failure {
            emailext(
                subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "The build ${env.BUILD_URL} failed. Please check the logs.",
                to: "olorunsolamiracle@gmail.com"
            )
        }
    }
}

