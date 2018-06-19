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
                         // mvn clean install
                echo 'This is a minimal pipeline.'
                 sh '''
                    mvn deploy release:clean release:prepare release:perform  war:war
                ''' 
            }
        }
    }
}