
pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Test Backend') {
            steps {
                dir('backend') {
                    sh 'mvn test'
                }
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'backend/target/*.jar',
                                 fingerprint: true
            }
        }
    }

    post {
        failure {
            emailext(
                to: 'TON_EMAIL@example.com',
                subject: "ECHEC Jenkins - ${JOB_NAME}",
                body: """
                Bonjour,

                La construction du backend a échoué.

                Projet : ${JOB_NAME}
                Build : ${BUILD_NUMBER}
                URL : ${BUILD_URL}

                Consultez Jenkins pour voir les erreurs.
                """
            )
        }
    }
}
