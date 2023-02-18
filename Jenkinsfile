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

            stage('Quality Gate Status'){
                steps{
                    script{
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-sec'
                    }
                }
            }
            stage('Upload war file to nexus'){
                steps{
                    script{

                        //def readPomVersion = readMavenPom file: 'pom.xml'
                        nexusArtifactUploader artifacts: [
                            [
                                artifactId: 'springboot',
                                classifier: '',
                                file: 'target/Uber.jar',
                                type: 'jar'
                                ]
                            ],
                             credentialsId: 'nexus-auth',
                             groupId: 'com.example',
                             nexusUrl: '172.18.0.2:8081',
                             nexusVersion: 'nexus3',
                             protocol: 'http',
                             repository: 'FirstProjectRelease/',
                             version: '1.0.1'
                    }
                }
            }
        }
}