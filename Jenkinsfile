pipeline {
    agent any  // Runs on any available agent (your Windows machine)

    tools {
        maven 'MAVEN_HOME'   // MUST match the name in Jenkins Maven installations
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
                            url: 'http://localhost:8000/')
                ],
                contextPath: '/sah',  // Your app's context path
                onFailure: false,  // Don't fail build on deploy error
                war: '**/*.war'  // Matches your target/*.war
            }
        }
    }
    
    // post {
    //     success {
    //         echo 'Deployment successful! App ready at http://localhost:8080/sah'
    //     }
    //     failure {
    //         echo 'Build/Deploy failed - check logs'
    //     }
    // }

    post {
        success {
            emailext(
                subject: "SUCCESS - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build passed successfully."
            )
        }
        failure {
            emailext(
                subject: "FAILURE - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build failed."
            )
        }
    }

}



// pipeline {
// agent any  // Agent: Defines the machine (or slave) that runs the tasks 
//    tools{
//         maven 'MAVEN_HOME'
//     }
//     stages {         //The script is divided into stages, each representing a specific step, like building,
//         stage('git repo & clean') {
//             steps {
//                 deleteDir()  // deletes entire workspace safely
//                 //bat "rmdir  /s /q mavenjava"
//                 bat "git clone https://github.com/NehaTadakamadla/Smart-Assessment-Hub.git"
//                 bat "mvn clean -f Smart-Assessment-Hub"
//             }
//         }
//         stage('install') {
//             steps {
//                 bat "mvn install -f Smart-Assessment-Hub"
//             }
//         }
//         stage('test') {
//             steps {
//                 bat "mvn test -f Smart-Assessment-Hub"
//             }
//         }
//         stage('package') {
//             steps {
//                 bat "mvn package -f Smart-Assessment-Hub"
//             }
//         }
//     }
// }
