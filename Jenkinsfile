pipeline {
    agent any

     environment
    {
    PATH="/usr/share/maven:$PATH"
   
    }
    

     stages {
        stage('git') {
            steps {
               git branch: 'main', credentialsId: '28a05ef9-3b3c-4466-847a-0a6a0edce8b5', url: 'https://github.com/monisha17068/spring.git'
            }
        }
        
         stage('build') {
            steps {
           sh 'mvn clean install -DskipTests'
        }
    }
      stage('sonar') 
          {
    
        steps
     {
        
   withSonarQubeEnv('sonarqube') {
           
   sh "mvn sonar:sonar \
  -Dsonar.projectKey=spring \
  -Dsonar.host.url=http://34.93.225.134:9000 \
  -Dsonar.login=2e950c5acae24d8f24bf8f8fbf8c293482fe771c  "
 
         }
        
         }
         }
         
    stage('docker') {
            steps {
                
           sh 'docker build -t springboot:1.2 .'
           sh 'docker run -d -p 8089:8080 springboot:1.2'
        }
    }
   
    }
    post {
        always {
            
            
            emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
            
        }
   
  }
}
