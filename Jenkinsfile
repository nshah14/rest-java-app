pipeline {
    agent any
    // tools { 
    //     maven 'maven 3.3.9' 
    // }
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
                    mvn clean install
                ''' 
            }
        }
    }
}