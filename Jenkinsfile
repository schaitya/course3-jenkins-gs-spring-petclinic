pipeline {
    agent {label "main"}
        options {
        timeout(time: 1, unit: 'HOURS') 
    }
    stages{
        
        stage("Package Maven"){
        steps{
                sh "pwd"
                sh "./mvnw clean package"
            }
        }
    
        stage("Sonar Analysis and Archive Package"){
            steps{
                    withSonarQubeEnv(installationName:'sonar-scanner' credentialsId: 'sonar1') {
                    sh "pwd"
                    sh """mvn sonar:sonar -Dsonar.url=https://localhost:9000/  -Dsonar.login=admin -Dsonar.password=sonar"""
                    script{
                        def qg = waitForQualityGate()
                        if(qg.status != 'OK'){
                            error "Pipeline Aborted because of quality issue"
                            }
                        }
                    }
                }
            post{
            success {
                    junit stdioRetention: '', testResults: '**/target/surefire-reports/TEST*.xml'
                    archiveArtifacts artifacts: '**/target/**.jar', followSymlinks: false
                }
            }
        }

        stage("Docker Build"){
            steps{
                sh "pwd"
                script{
                    withDockerRegistry(credentialsId: '12e3ac3b-7e4e-473d-8fcf-bb60074fd35f',  url: 'https://registry.hub.docker.com') { 
                        sh "sudo docker build -f Dockerfile -t petclinic:latest ."
                        sh "sudo docker tag petclinic:latest schaitya47/petclinic:latest"
                        sh "sudo docker push schaitya47/petclinic:latest"
                    }
                }
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
