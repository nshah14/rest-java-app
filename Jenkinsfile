#!/usr/bin/env groovy
pipeline {

    agent { label 'build' }
    environment {
        //Use Pipeline Utility Steps plugin to read information from pom.xml into env variables
        IMAGE = readMavenPom().getArtifactId()
        VERSION = readMavenPom().getVersion()
        BUILD_RELEASE_VERSION = readMavenPom().getVersion().replace("-SNAPSHOT", ".1.1")
        IS_SNAPSHOT = readMavenPom().getVersion().endsWith("-SNAPSHOT")
        GIT_TAG_COMMIT = sh(script: 'git describe --tags --always', returnStdout: true).trim()
        // writeMavenPom().setVersion("4.1.2")
        NEW_VERSION = readMavenPom().getVersion()
    }
    tools { 
        jdk 'jdk'
        maven 'maven 3.5.3' 
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "git branch"
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
              
                echo " Project version is ${VERSION}"
                echo "Artifact id is ${IMAGE}"
                echo "Build release version is ${BUILD_RELEASE_VERSION}"
                echo " is it snapshot ${IS_SNAPSHOT}"
                echo " is GIT_TAG_COMMIT ${GIT_TAG_COMMIT}"
               
  
               
                 sh '''
                    mvn versions:set -DnewVersion=1.0.1
                    mvn clean install

                ''' 
                script{
                    environment {
                        NEW_VERSION = readMavenPom().getVersion()
                        echo " Project new  version is ${NEW_VERSION}"
                    }
                }
            }
        }

        stage('Deploy To Integration') {
            when { tag "release-*" }
            steps{
                echo 'start'
                script{
                def userWantToKeepCluster = {
                    
                        try {
                            timeout(time: 1, unit: 'MINUTES') {
                                def keep = input message: 'Deploy poa-bal ?', 
                                            parameters: [booleanParam(defaultValue: true, description: 'Make sure to destroy cluster manually after you done', name: 'Deploy poa-bal')]
                                return keep
                            }
                        } catch(e) {
                            echo "Build Failed ::: User aborted build or its timed out"
                            throw e
                        }
                }
                  def  answer = userWantToKeepCluster()
                  echo "will keep cluster? $answer()"
            
                    if(answer)
                        {
                            echo "answer is $answer"
                            
                            sh '''
                             mvn  deploy

                            ''' 
                        }
                    else{
                        echo "Build is aborted, User does not want to deploy artifact"
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
 
        