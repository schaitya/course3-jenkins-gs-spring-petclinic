pipeline {
    agent any
    stages{
        
        stage("Complie Maven"){
        steps{
                sh "pwd"
                sh "./mvnw clean package"
            }
        }
    
        stage("Unit Testing"){
        steps{
                junit stdioRetention: '', testResults: '**/target/surefire-reports/TEST*.xml'
            }
        }

        stage("Sonar Analysis"){
            steps{
                    sh "pwd"
                    sh """mvn sonar:sonar -Dsonar.url=https://localhost:9000/  -Dsonar.login=admin -Dsonar.password=sonar
                """
            }
        }

        stage("Archive Build"){
            steps{
                archiveArtifacts artifacts: '**/target/**.jar', followSymlinks: false
            }
        }
        stage("Trigger Deployment Build"){
            steps{
                build 'cd-final-project'
            }
        }
    }
    post {
        always {
                 emailext body: """Hi Team,
Project Name : ${currentBuild.projectName} 
Build No. : ${currentBuild.number} 
Build Status  : ${currentBuild.currentResult} 
Duration : ${currentBuild.durationString} 
URL : ${currentBuild.absoluteUrl} 

Regards,
Jenkins Pipeline""", recipientProviders: [developers()], 
subject: " ${currentBuild.projectName} build status report: ${currentBuild.currentResult}", 
to: 'chaityashah89155@gmail.com'
        }
    }
}
