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
        JIRA_SITE='JIRA'
        GIT_COMMIT_MSG = sh(script: 'git log -1 --oneline')
        GIT_COMMIT_AUTHOR = sh(script: 'git log --format="medium" -1 ${GIT_COMMIT}')
        GIT_COMMIT_PRETTY = sh(script: 'git log -1 --pretty=%B' , returnStdout: true).trim()
        // commitHash = checkout(scm).GIT_COMMIT?
        // sh "echo 'Commit hash is: ${commitHash}'"
    }
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
              
                echo " Project version is ${VERSION}"
                echo "Artifact id is ${IMAGE}"
                echo "Build release version is ${BUILD_RELEASE_VERSION}"
                echo " is it snapshot ${IS_SNAPSHOT}"
                echo " is GIT_TAG_COMMIT ${GIT_TAG_COMMIT}"
                echo "Commit msg is: ${GIT_COMMIT_MSG}"
                // echo "author hash is: ${GIT_COMMIT_AUTHOR}"
                echo "author hash is: ${GIT_COMMIT_PRETTY}"
  
               
                 sh '''
                   
                    mvn versions:set -DnewVersion=1.0.2
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
        
        stage('JIRA') {
            steps{
                 script{
                        def serverInfo = jiraGetServerInfo()
                        echo serverInfo.data.toString()
                        // def searchResults = jiraJqlSearch jql: "project = TEST AND issuekey = 'TEST-3'"
                        // def issues = searchResults.data.issues
                        // for (i = 0; i <issues.size(); i++) {
                        //     def fixVersion =  jiraNewVersion version: [name: "new-fix-version-3.0",
                        //                                                 project: "TEST"]
                        //     def testIssue = [fields: [fixVersions: [fixVersion.data]]]
                        //     response = jiraEditIssue idOrKey: issues[i].key, issue: testIssue
                        // }

                        // def testIssue = [fields: [ // id or key must present for project.
                        // project: [id: '10000'],
                        // summary: 'New JIRA Created from Jenkins.',
                        // description: 'New JIRA Created from Jenkins.',
                        // // id or name must present for issuetype.
                        // //issuetype: [id: '3']
                        // ]]

                        // response = jiraEditIssue idOrKey: 'TEST-2', issue: testIssue

                        // echo response.successful.toString()
                        // enscho response.data.toString()
                        
               
                        println "${GIT_COMMIT_PRETTY}".tokenize("-")
                        "${GIT_COMMIT_PRETTY}".tokenize(",").each {


                                println "Number ${it}"
                                def searchResults = jiraJqlSearch jql: "project = TEST AND issuekey = '${it}'"
                                def issues = searchResults.data.issues
                                for (i = 0; i <issues.size(); i++) {
                                        def fixVersion =  jiraNewVersion version: [name: "new-fix-version-4.0",
                                                                        project: "TEST"]
                                        echo " version : "+fixVersion.data
                                        def testIssue = [fields: [fixVersions: [fixVersion.data]]]

                                    // def fixVersion = [name: "new-fix-version-3.0",
                                    //                                             project: "TEST"]
                                    // def testIssue = [fields: [fixVersions: [name: "new-fix-version-3.0", project: "TEST" ]]]
                                    // response = jiraEditIssue idOrKey: issues[i].key, issue: testIssue
                                }
                                // def transitions = jiraGetIssueTransitions idOrKey: "${it}"
                                // echo transitions.data.toString()
                                // def transitionInput = [ transition: [ id: '31'] ]
                                // jiraTransitionIssue idOrKey: "${it}", input: transitionInput, site: 'JIRA'
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
 
        