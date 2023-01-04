pipeline {
    agent any



    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/Minimacaco59/Integration_continue_TP001.git'

                // Run Maven on a Unix agent.
                bat "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        
        stage('Analyse'){
            steps{
                bat "mvn checkstyle:checkstyle"
                bat "mvn spotbugs:spotbugs"
                bat "mvn pmd:pmd"
            }
        }

        stage('Publish') {
            steps{
                archiveArtifacts 'target/*.jar'
            }
        }
    }
    
    post {
        always{
            junit '**/surefire-reports/*.xml'
            recordIssues enabledForFailure: true, tools: [mavenConsole(), java(), javaDoc()]
            recordIssues enabledForFailure: true, tool: checkStyle()
            recordIssues enabledForFailure: true, tool: spotBugs()
            recordIssues enabledForFailure: true, tool: cpd(pattern: '**/target/cpd.xml')
            recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
        }           
    }
}
