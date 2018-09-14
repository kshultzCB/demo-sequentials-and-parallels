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
                        echo "This works because we create it immediately."
                        echo "Uncomment the sleep 5, one line down, and it'll fail."
                        // sleep 5 
                        sh "touch parallel-file-2"
                    }
                }
                stage('Look for parallel-file-2') {
                    steps {
                        echo "This will work because the file was created first."
                        echo "But if we remove the sleep 2, it might not."
                        sleep 2
                        sh "ls -alh --time-style=full-iso parallel-file-2"
                    }
                }
            }
        } 
    } // ends stages
}
