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
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-cred2'
                    }
                }
            }
            stage('Upload war file to nexus'){
                steps{
                    script{

                        def readPomVersion = readMavenPom file: 'pom.xml'
                        def NexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "FirstProjectSnapShot/" : "FirstProjectRelease/"
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
                             nexusUrl: '172.17.0.2:8081',
                             nexusVersion: 'nexus3',
                             protocol: 'http',
                             repository: NexusRepo,
                             version: readPomVersion.version
                    }
                }
            }
            stage('Docker image build'){
                steps{
                    script{
                        sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID khalabi/$JOB_NAME:v1.$BUILD_ID'
                        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID khalabi/$JOB_NAME:latest'
                    }
                }
            }
            stage('Push image to DockerHub'){
                steps{
                    script{
                        withCredentials([string(credentialsId: 'docker-cred', variable: 'docker-hub-cred')]) {
                            sh 'docker login -u khalabi -p $docker-hub-cred'
                            sh 'docker image push khalabi/$JOB_NAME:v1.$BUILD_ID'
                            sh 'docker image push khalabi/$JOB_NAME:latest'
                        }
                        
                    }
                }
            }
        }
}