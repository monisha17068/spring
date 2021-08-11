pipeline {
    agent any

    

    environment {
        
         PATH="/usr/share/maven:$PATH"

       

    }

    stages {

        stage("Clone") {

            steps {

               git branch: 'main', credentialsId: '28a05ef9-3b3c-4466-847a-0a6a0edce8b5', url: 'https://github.com/monisha17068/spring.git'

            }

        }

        stage("Maven Build") {

            steps {

                script {

                    sh "mvn package -DskipTests=true"

                }

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

                      
                        nexusArtifactUploader artifacts: [[
                            artifactId: 'spring-boot-data-jpa',
                            classifier: '', file: 'target/spring-boot-data-jpa.jar',
                            type: 'jar']], 
                            credentialsId: 'nexus-credentials', 
                            groupId: 'org.springframework.boot', 
                            nexusUrl: 'http://35.244.12.68:8081/',
                            nexusVersion: 'nexus3',
                            protocol: 'http',
                            repository: 'http://35.244.12.68:8081/repository/spring/', 
                            version: '0.0.1'
                    }
                }
            }
        
                        else {

                        error "*** File: ${artifactPath}, could not be found";

                    }

                }

            }

        }

    }

}

