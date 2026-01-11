pipeline {
    agent any
    
    environment {
        JAVA_HOME = '/usr/lib/jvm/java-11-openjdk-amd64'
        PATH = "${JAVA_HOME}/bin:${PATH}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                git branch: 'main', url: 'https://github.com/Fahssi-nabil/Projet-DevOps-NabilFahssi.git'
                sh 'git clean -fd'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building the application with Maven...'
                sh 'mvn clean compile'
                sh 'mvn test'
            }
            post {
                success {
                    echo 'Build and tests passed successfully!'
                }
                failure {
                    echo 'Build or tests failed!'
                }
            }
        }
        
        stage('Package') {
            steps {
                echo 'Packaging the application...'
                sh 'mvn package -DskipTests'
            }
        }
        
        stage('Archive') {
            steps {
                echo 'Archiving build artifacts...'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                archiveArtifacts artifacts: 'target/surefire-reports/**/*', allowEmptyArchive: true
            }
        }
        
        stage('Deploy') {
            when {
                expression { 
                    currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                echo 'Deployment step (optional - can be configured for local or cloud deployment)'
                // Exemple de déploiement local
                // sh 'cp target/*.jar /opt/deployment/'
            }
        }
        
        stage('Notify Slack') {
            steps {
                echo 'Sending notification to Slack...'
                script {
                    def message = """
                    *Pipeline ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}*
                    Status: *${currentBuild.result ?: 'SUCCESS'}*
                    Branch: *${env.GIT_BRANCH}*
                    Commit: *${env.GIT_COMMIT.take(7)}*
                    Console: ${env.BUILD_URL}
                    """
                    slackSend(
                        color: currentBuild.result == 'SUCCESS' ? 'good' : 'danger',
                        message: message,
                        channel: '#devops-notifications'
                    )
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed. Cleaning up...'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
