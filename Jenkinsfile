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
            steps {
                agent 'build' {
                    echo "Static !"
                    sleep 5
                }
            }
        }
        stage('acceptance-tests') {
            steps {

                parallel chrome: {
                    agent "test" {
                        echo "acceptance - chrome"
                    }
                },
                        edge: {
                            agent("test") {
                                echo "acceptance - edge"
                            }
                        },
                        end: {
                            agent("test") {
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
