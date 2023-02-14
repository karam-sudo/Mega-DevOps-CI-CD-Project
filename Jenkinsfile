pipeline {
    agent any

        stages{
            stage('Git Checkout'){
                steps{
                    script{
                        git branch: 'main', url: 'https://github.com/karam-sudo/Mega-DevOps-CI-CD-Project.git'
                    }
            }
        }

            stage('UNIT Test'){
                steps{
                    script{
                        sh 'mvn test'
                    }
                }
            }
            stage('Integration testing'){
                
                steps{
                    
                    script{
                        
                        sh 'mvn verify -DskipUnitTests'
                    }
                }
            }
            stage('Maven build'){
            
                steps{
                    
                    script{
                        
                        sh 'mvn clean install'
                    }
                }
            }
            stage('SonarQube analysis'){
                
                steps{
                    
                    script{
                        
                        withSonarQubeEnv(installationName: 'sonarqube') {
                            sh 'mvn clean package sonar:sonar'
                        }
                    }
                        
                }
            }

            stag('Quality Gate Status'){
                steps{
                    script{
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-sec'
                    }
                }
            }
        }
}