pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/aishwaryaital95/demo-counter-app.git'
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
                    
                    withSonarQubeEnv(credentialsId: 'sonar-qube-api') {
                        
                        sh 'mvn clean package sonar:sonar'
                    }
                   }
                    
                }
            
  }
 stage('Quality Gate Status'){
                
                steps{
                    
                    script{
                        
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-qube-api'

                        
                    }
                }
                
                
                
                
            }
    


stage('upload war file to nexus'){
                
                steps{
                    
                    script{
                        
                      def readPomversion = readMavenPom file: 'pom.xml' 

                       
                nexusArtifactUploader artifacts:
                 [
                    [artifactId: 'springboot',
                     classifier: '', file: 'target/Uber.jar',
                      type: 'jar']
                      ],
                      
                       credentialsId: 'nexus-auth', 
                       groupId: 'com.example', 
                       nexusUrl: '34.224.166.18:8081',
                        nexusVersion: 'nexus3', 
                        protocol: 'http',
                         repository: 'demoapp-SNAPSHOT', 
                         version: "${readPomversion.version}"

                        
                    }
                }


}
}}

