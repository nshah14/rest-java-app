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
                         //mvn deploy war:war release:clean release:prepare release:perform  
                echo 'This is a minimal pipeline.'
                 sh '''
                    mvn clean install

                ''' 
            }
        }
        stage('Promote Build') {
            input {
                message "Should we continue?"
                ok "Yes, we should."
                // parameters {
                //     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                // }
            }
            steps {
                echo "Hello, execute, nice to meet you."
                echo 'This is a minimal pipeline.'
                 sh '''
                    mvn clean install

                ''' 
            }
        }
    }
}