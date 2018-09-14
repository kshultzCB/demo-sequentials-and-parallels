pipeline {

    agent any

    stages {
        stage('Sequential Stages') {
            stages {
                stage('1. touch file-1 then sleep') { 
                    steps {
                        sh "touch sequential-file-1"
                    }
                }
                stage ('2. sleep for 2s because otherwise this goes too fast') {
                    steps {
                        sh "sleep 2"
                    }
                }
                stage('3. touch file-2') {
                    steps {
                        sh "touch sequential-file-2"
                    }
                }
                stage('4. Look at timestamps on the files') {
                    steps {
                        echo "Should be about 2s difference in these two files"
                        sh "ls -alh --time-style=full-iso sequential*"
                    }
                }
            }
        }


        stage('Parallel Stages') {
            parallel {
                stage('Create parallel-file-2 immediately') { 
                    steps {
                        echo "This purposely sets up a failure to demonstrate parallel stage execution."
                        echo "The sleep 5 means our file will not exist in time for the next stage to find it."
                        sleep 5 
                        sh "touch parallel-file-2"
                    }
                }
                stage('Look for parallel-file-1') {
                    steps {
                        echo "This will fail because it is running at the same time, but the sleep 5"
                        echo "in the previous stage is preventing the file from being created in time."
                        sh "ls -alh --time-style=full-iso parallel-file-2"
                    }
                }
            }
        } 
    } // ends stages
}
