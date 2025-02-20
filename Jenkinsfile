pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Maven"
    }

    environment {
        JACOCO_PATH = "${WORKSPACE}/target/site/jacoco"
    }

    triggers {
        cron('H/10 * * * 1')  // Every 10 minutes on Mondays
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/vdnnguyen94/spring-petclinic'

                // Run Maven Build
                bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }

        stage('Test & Code Coverage') {
            steps {
                // Run tests with JaCoCo
                bat "mvn test"
                jacoco execPattern: '**/target/jacoco.exec'
            }
        }
    }

    post {
        always {
            // Publish test results
            junit '**/target/surefire-reports/TEST-*.xml'

            // Publish JaCoCo report
            jacoco execPattern: '**/target/jacoco.exec'
        }
    }
}
