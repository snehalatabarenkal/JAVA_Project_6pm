pipeline{
    agent any
    
    tools {
        maven 'mymaven'
            }
    
    stages{
        
        stage('Git Clone'){
            
            steps{
               git branch: 'main', url: 'https://github.com/snehalatabarenkal/JAVA_Project_6pm'
            }
        }
        
        stage("SonarQube_check_quality"){
            
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonartoken') {
                      sh 'mvn sonar:sonar'
                      
                   }
                   def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    sh "mvn clean install"
                  }
               }
             }  
        
        stage('Docker Build'){
            steps{
                sh 'docker build -t snehalatabarenkal/JAVA_Project_6pm .'
            }
        }
        
        stage('Docker Push'){
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerpush')]) {
                sh 'docker login -u nbktechnosys -p ${dockerpush}'
                }
                 sh 'docker push snehalatabarenkal/JAVA_Project_6pm/myjavaproject6jun'
            }
           
        }
    }
}
