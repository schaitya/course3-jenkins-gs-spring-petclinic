pipeline {
    agent any
    stages{
        
        stage("Complie Maven"){
        steps{
        sh "./mvnw build"
            }
        }
    
        stage("Unit Testing"){
        steps{
        junit stdioRetention: '', testResults: '**/target/surefire-reports/TEST*.xml'
            }
        }
    }
}
