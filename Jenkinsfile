pipeline {
    agent any

     environment
    {
    PATH="/usr/share/maven:$PATH"
    NEXUS_VERSION = "nexus3"
    NEXUS_PROTOCOL = "http"
    NEXUS_URL = "34.93.225.134"
    NEXUS_REPOSITORY = "spring"
    NEXUS_CREDENTIAL_ID = "nexus-credentials"    
   
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
      
           stage("Publish to Nexus Repository Manager") {
31
            steps {
32
                script {
33
                    pom = readMavenPom file: "pom.xml";
34
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
35
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
36
                    artifactPath = filesByGlob[0].path;
37
                    artifactExists = fileExists artifactPath;
38
                    if(artifactExists) {
39
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
40
                        nexusArtifactUploader(
41
                            nexusVersion: NEXUS_VERSION,
42
                            protocol: NEXUS_PROTOCOL,
43
                            nexusUrl: NEXUS_URL,
44
                            groupId: pom.groupId,
45
                            version: pom.version,
46
                            repository: NEXUS_REPOSITORY,
47
                            credentialsId: NEXUS_CREDENTIAL_ID,
48
                            artifacts: [
49
                                [artifactId: pom.artifactId,
50
                                classifier: '',
51
                                file: artifactPath,
52
                                type: pom.packaging],
53
                                [artifactId: pom.artifactId,
54
                                classifier: '',
55
                                file: "pom.xml",
56
                                type: "pom"]
57
                            ]
58
                        );
59
                    } else {
60
                        error "*** File: ${artifactPath}, could not be found";
61
                    }
62
                }
63
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
