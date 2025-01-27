pipeline {
    agent any
    
    stages {
        
        stage('Database Operations') {
            steps {
                script {
                    // Backup the current data in the database
                    sh 'sqlite3 Employees.db .dump > dump.sql'

                    // Remove the existing database file
                    sh 'rm Employees.db'

                    // Create a new database file and load the new schema from the SQL file
                    sh 'sqlite3 Employees.db < sqlite.sql'

                    // Restore the previously backed up data
                    sh 'cat dump.sql | awk \'/^INSERT.*;$/\' | sqlite3 Employees.db'
                }
            }

            post {
                success {
                    echo 'Database operations completed successfully!'
                }
                failure {
                    echo 'Error occurred during database operations!'
                    sh 'rm Employees.db'
                    sh 'sqlite3 Employees.db < dump.sql'
                }
                aborted {
                    echo 'Pipeline aborted!'
                    sh 'rm Employees.db'
                    sh 'sqlite3 Employees.db < dump.sql'
                }
                cleanup {
                    sh 'rm dump.sql'
                }
            }
        }
    }
}
