pipeline {
    agent any
    tools { 
        maven 'MAVEN_HOME'
   jdk 'jdk1.8'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64"
                    echo "MAVEN_HOME = $/usr/share/maven"

                ''' 
            }
        }

        stage ('Build') {
            steps {
                echo 'This is a minimal pipeline.'
            }
        }
    }
}
