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
                        export HOME="$PWD"
                        export npm_config_cache="$PWD/.npm-cache"
                        mkdir -p "$npm_config_cache"
                        # clean old dependency
                        rm -rf node_modules
                        npm ci --no-audit --no-fund
                        npm run build

                        ls -la
                   '''
            }
        }

        stage("Running Test command") {
            
            steps {
                sh '''
                    test -f build/index.html
                    echo "Running test command ******** "
                    CI=true npm test -- --watchAll=false
                   '''
            }
        }
    }

    post {
        always {
            junit 'test-reaults/junit.xml'
        }
    }
}