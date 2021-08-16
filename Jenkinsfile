pipeline {
    agent any

    

    environment {
        
     PATH="/usr/share/maven:$PATH"
    NEXUS_VERSION = "nexus3"
    NEXUS_PROTOCOL = "http"
    NEXUS_URL = "35.244.12.68:8081"
    NEXUS_REPOSITORY = "spring"
    NEXUS_CREDENTIAL_ID = "nexus-credentials"    
       

    }

    stages {

        stage("Clone") {

            steps {

               git branch: 'main', credentialsId: '28a05ef9-3b3c-4466-847a-0a6a0edce8b5', url: 'https://github.com/monisha17068/spring.git'

            }

        }
         stage("sonar") {

            steps {
       sh' mvn sonar:sonar \
  -Dsonar.projectKey=springnew \
  -Dsonar.host.url=http://34.93.225.134:9000 \
  -Dsonar.login=ce8b3b76b85bb42ef26c8e8fd9de8907a082819a '
            }
         }

        stage("Maven Build") {

            steps {

                  sh "mvn package -DskipTests=true"

                

            }

        }
       

        stage("Publish to Nexus Repository Manager") {

            steps {

                 script {
                   pom = readMavenPom file: "pom.xml";
                   
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                   
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    
                    artifactPath = filesByGlob[0].path;
                  
                    artifactExists = fileExists artifactPath;

                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";

                     
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],

                                 
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );

                    } 
              
                
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

    
}


