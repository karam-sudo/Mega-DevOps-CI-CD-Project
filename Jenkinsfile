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
    }
}