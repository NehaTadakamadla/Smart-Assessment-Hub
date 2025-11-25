pipeline {
    agent any  // Runs on any available agent (your Windows machine)

    tools {
        maven 'Maven3'   // MUST match the name in Jenkins Maven installations
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master',
                    credentialsId: 'Githubtoken3',  // Your GitHub credential ID from Jenkins
                    url: 'https://github.com/NehaTadakamadla/Smart-Assessment-Hub.git'
            }
        }
        
        stage('Build with Maven') {
            steps {
                bat 'mvn clean package'  // Use 'sh' if Linux; 'bat' for Windows
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [
                    tomcat9(credentialsId: '6b09b412-3364-49ce-969e-56d1f9f582a4',  // Create this credential in Jenkins (see below)
                            url: 'http://localhost:8080/')
                ],
                contextPath: '/sah',  // Your app's context path
                onFailure: false,  // Don't fail build on deploy error
                war: '**/*.war'  // Matches your target/*.war
            }
        }
    }
    
    post {
        success {
            echo 'Deployment successful! App ready at http://localhost:8080/sah'
        }
        failure {
            echo 'Build/Deploy failed - check logs'
        }
    }
}
