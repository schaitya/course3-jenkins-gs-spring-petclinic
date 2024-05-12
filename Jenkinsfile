pipeline {
    agent any
    stages{
        
        stage("Complie Maven"){
        steps{
                sh "./mvnw package"
            }
        }
    
        stage("Unit Testing"){
        steps{
                junit stdioRetention: '', testResults: '**/target/surefire-reports/TEST*.xml'
            }
        }
        stage("Archive Build"){
            steps{
                archiveArtifacts artifacts: '**/target/**.jar', followSymlinks: false
            }
        }
    }
}
