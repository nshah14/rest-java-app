pipeline {
    agent { label 'build' }
    tools { 
        jdk 'jdk'
        maven 'maven 3.5.3' 
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }

        stage ('Build') {
            steps {
                echo 'This is a minimal pipeline.'
                 sh '''
                    // mvn clean instal
                    mvn deploy clean:release release:prepare release:perform
                ''' 
            }
        }
    }
}