pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/Mbianda2022/mbianda_java_application_2.git'
                }
            }
        }
        stage('UNIT testing'){
            
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
        stage('Static code analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonarQube-api') {
                        
                        sh 'mvn clean package sonar:sonar'
                    }
                   }
                    
                }
            }
            stage('Quality Gate Status'){
                
                steps{
                    
                    script{
                        
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonarQube-api'
                    }
                }
            }
            stage('nexus artifact'){

                steps{

                    script{

                        nexusArtifactUploader artifacts: 
                        [
                            [
                                
                               artifactId: 'springboot', 
                               classifier: '', 
                               file: 'target/Uber.jar', 
                               type: 'jar'
                             ]
                          
                        ], 
                          credentialsId: 'nexus-credentials', 
                          groupId: 'com.example', 
                          nexusUrl: '44.201.105.157:8081', 
                          nexusVersion: 'nexus3', 
                          protocol: 'http', 
                          repository: 'cloudlord-release', 
                          version: '1.0.0'
                    }
                }
            }
        }
        
}