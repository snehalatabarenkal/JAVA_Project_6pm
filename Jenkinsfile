pipeline{
    agent any
    
    tools {
        maven 'mymaven'
            }
    
    stages{
        
        stage('Git Clone'){
            
            steps{
               git branch: 'main', url: 'https://github.com/knbtechnosys/JAVA_Project_6pm.git'
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
                sh 'docker build -t nbktechnosys/myjavaproject6un .'
            }
        }
        
        stage('Docker Push'){
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerpush')]) {
                sh 'docker login -u nbktechnosys -p ${dockerpush}'
                }
                 sh 'docker push nbktechnosys/myjavaproject6jun'
            }
           
        }
    }
}
