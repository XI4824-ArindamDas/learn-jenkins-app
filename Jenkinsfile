pipeline {
    agent any 

    stages {
        stage("Build") {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                        ls -la
                        npm --version
                        node --version
                        export npm_config_catch="$PWD/.npm-catch"
                        npm config set cache "$npm_config_cache" --global || true
                        npm install
                        npm run build
                        ls -la
                   '''
            }
        }

        stage("Running Test command") {
            steps {
                sh '''
                    echo "Running test command"
                  '''
            }
        }
    }
}