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

        stage('Run E2E Test') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.58.2-noble'
                    reuseNode true
                }
            }

            steps {
                sh ''' 
                    npx serve -s build &
                    sleep 13
                    npx playwright test
                   '''
            }
        }
    }

    post {
        always {
            sh '''
                 echo "$PWD"
                 ls
               '''
            junit 'test-results/junit.xml'
        }
    }
}