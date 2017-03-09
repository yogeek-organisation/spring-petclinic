pipeline {
    agent none

    stages {

        stage('build & unit tests') {
            steps {
                agent 'build' {
                    echo "Build !"
                    sleep 5
                }
            }
        }

        stage('static-analysis') {
            agent {
                label 'build'
            }
            steps {

                echo "Static !"
                sleep 5
            }
        }
    }
    stage('acceptance-tests') {
        steps {

            parallel chrome: {
                node "test" {
                    echo "acceptance - chrome"
                }
            },
                    edge: {
                        node("test") {
                            echo "acceptance - edge"
                        }
                    },
                    end: {
                        node("test") {
                            echo "acceptance - firefox"
                        }
                    }
        }

    }


    stage('manual-input') {
        steps {
            input "T'es sûr tu veux déployer?"
        }

    }

}

}
