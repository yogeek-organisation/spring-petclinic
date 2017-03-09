pipeline {

    agent none

    stages {

        stage('build & unit tests') {
            agent { label 'build' }
            steps {
                withMaven(maven: 'M3') { // Maven installation declared in the Jenkins "Global Tool Configuration"
                    // Run the maven build
                    sh "mvn clean install"
                }
                sh "ls -la ${pwd()}"
            }
            post {
                success {
                    sh "ls -la ${pwd()}"
                    // PWD = /jenkins/workspace/-petclinic_Jenkinsification-7V7AKFPANGYA7IB55PLXWGSKEUVY4RQGBTDNMX55WKQ4EXSXYJQA
                    stash name: 'mvn-result', includes: 'target/*.jar'
                }
            }
        }

        stage('static-analysis') {
            agent { label "build "}
            steps {
                withMaven(maven: "M3") {
                    withSonarQubeEnv("sonarqube") {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }

        stage('acceptance-tests') {
            steps {
                parallel(
                        "chrome": {
                            node(label: 'test') {
                                sh 'echo "in parallel test agent !"'
                            }
                        },
                        "edge": {
                            node(label: 'test') {
                                sh 'echo "in parallel test agent !"'
                            }
                        },
                        "firefox": {
                            node(label: 'test') {
                                sh 'echo "in parallel test agent !"'
                            }
                        }
                )
            }
        }

        stage('staging') {
            agent {
                label 'build'
            }
            steps {

                echo "Staging !"
                sleep 5

                deleteDir() // clean workspace
                unstash 'mvn-result' // unstash
                sh "ls -l target" // check if unstashed dir exists
            }
        }

        stage('manual-input') {
            steps {
                input "T'es sûr tu veux déployer?"
            }

        }

        stage('deploy') {
            agent {
                label 'build'
            }
            steps {

                echo "Deploy !"
                sleep 5
            }
        }

    }
}
