pipeline {
    agent any

    tools {
        maven 'Maven 3.5.2'
        jdk 'jdk1.8.0_151'
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Tests') {
            parallel {

                stage('Tests Unitaires') {
                    steps {
                        sh 'mvn test'
                    }
                    post {
                        always {
                            junit 'target/surefire-reports/**/*.xml'
                        }
                    }
                }

                stage('Couverture de Code') {
                    steps {
                        sh 'mvn cobertura:cobertura'
                    }
                }

                stage('Documentation') {
                    steps {
                        sh 'mvn site'
                    }
                }
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
            post {
                always {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'mvn deploy -DskipTests'
            }
        }
    }

    post {
        failure {
            mail to: 'admin@example.com',
                 subject: "Echec Build :  #",
                 body: "Erreur dans le build. Verifiez : "
        }
        success {
            echo 'Pipeline termine avec succes !'
        }
    }
}
