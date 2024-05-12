pipeline {
    agent any
    stages{
        
        stage("Complie Maven"){
        steps{
                sh "pwd"
                sh "./mvnw package"
            }
        }
    
        stage("Unit Testing"){
        steps{
                junit stdioRetention: '', testResults: '**/target/surefire-reports/TEST*.xml'
            }
        }

        stage("Sonar Analysis"){
            steps{
                    sh """mvn sonar:sonar -Dsonar.url=https://localhost:9000/  -Dsonar.login=admin -Dsonar.password=sonar
                    -Dsonar.projectName=course3-jenkins-gs-spring-petclinic
                    -Dsonar.java.binaries=.\
                    -Dsonar.projectKey=course3-jenkins-gs-spring-petclinic
                """
            }
        }

        stage("Archive Build"){
            steps{
                archiveArtifacts artifacts: '**/target/**.jar', followSymlinks: false
            }
        }
    }
}
