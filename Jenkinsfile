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

        stage('UA') {
            steps{
                echo 'start'
                script{
                def userWantToKeepCluster = {
                    
                        try {
                            timeout(time: 1, unit: 'MINUTES') {
                                def keep = input message: 'Keep cluster?', 
                                            parameters: [booleanParam(defaultValue: true, description: 'Make sure to destroy cluster manually after you done', name: 'keepCluster')]
                                return keep
                            }
                        } catch(e) {
                            return "timeout"
                        }
                }
                  def  answer = userWantToKeepCluster()
                  echo "will keep cluster? $answer()"
            
                     if(answer)
                        {
                            sh '''
                            mvn clean install

                            ''' 
                        }
                    else if(answer == "timeout"){
                            error('Stopping early…')
                        }
                    else{
                        echo "Build is aborted"
                    }
                  }
                
                
                
                echo 'done'
            }
               
        }

        // stage('Promote Build') {
        //     input {
        //         message "Do you want to release"
        //         ok "Yes"
        //         // parameters {
        //         //     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
        //         // }
        //     }
        //     steps {
        //         echo "Hello, execute, nice to meet you."
        //         echo 'This is a minimal pipeline.'
        //          sh '''
        //             mvn clean install

        //         ''' 
        //     }
        // }
    }
    
}
 
        