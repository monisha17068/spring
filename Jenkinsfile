pipeline {
    agent any
    tools { 
        maven 'MAVEN_HOME'
   jdk 'JAVA_HOME'
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
                sh 'mvn -Dmaven.test.failure.ignore=true install' 
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml' 
                }
            }
        }
    }
}
