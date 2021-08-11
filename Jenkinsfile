pipeline {
    agent {
3
        label "main"
4
    }
5
    
8
    environment {
        
         PATH="/usr/share/maven:$PATH"

        NEXUS_VERSION = "nexus3"
10
        NEXUS_PROTOCOL = "http"
11
        NEXUS_URL = "http://35.244.12.68:8081/"
12
        NEXUS_REPOSITORY = "spring"
13
        NEXUS_CREDENTIAL_ID = "nexus-credentials"
14
    }
15
    stages {
16
        stage("Clone") {
17
            steps {
18
                script {
19
                    git 'https://github.com/monisha17068/spring';
20
                }
21
            }
22
        }
23
        stage("Maven Build") {
24
            steps {
25
                script {
26
                    sh "mvn package -DskipTests=true"
27
                }
28
            }
29
        }
30
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

                       nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                // Artifact generated such as .jar, .ear and .war files.
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],

                                // Lets upload the pom.xml file for additional information for Transitive dependencies
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );

                    } 
                        else {
60
                        error "*** File: ${artifactPath}, could not be found";
61
                    }
62
                }
63
            }
64
        }
65
    }
66
}

