pipeline {
    agent any
    stages{
        
        stage("Complie Maven"){
        steps{
        sh "./mvnw clean"
            }
        }
    
        stage("Unit Testing"){
        steps{
        junit stdioRetention: '', testResults: '**/target/surefire-reports/TEST*.xml'
            }
        }
    }
}
