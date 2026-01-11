pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                git branch: 'main', url: 'https://github.com/Fahssi-nabil/Projet-DevOps-NabilFahssi.git'
                bat 'git clean -fd'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building the application with Maven...'
                bat 'mvn clean compile'
                bat 'mvn test'
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
                bat 'mvn package -DskipTests'
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
                script {
                    try {
                        echo 'Sending notification to Slack...'
                        // Note: Slack notification nécessite le plugin Slack Notification configuré
                        // Si le plugin n'est pas configuré, cette étape sera ignorée
                        if (currentBuild.result == null || currentBuild.result == 'SUCCESS') {
                            echo 'Build réussi! (Notification Slack optionnelle - configurez Slack si nécessaire)'
                        } else {
                            echo 'Build échoué! (Notification Slack optionnelle - configurez Slack si nécessaire)'
                        }
                        // Décommentez et configurez Slack pour activer les notifications:
                        /*
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
                        */
                    } catch (Exception e) {
                        echo "Notification Slack non disponible: ${e.message}"
                        echo 'Continuez sans notification Slack'
                    }
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
