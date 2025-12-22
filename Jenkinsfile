pipeline {
    agent any

environment {
    NETLIFY_SITE_ID='b61f91b6-ede6-4ab7-9d62-f6cdd408fa3a'
}
    stages {
        stage('Clean') {
            steps {
                sh '''
                echo "Cleaning up npm environment..."
                npm cache clean --force
                '''
            }
        }
        stage('Tests') {
            parallel{
                stage ('Unit test') {
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                        test -f build/index.html
                        npm test
                        '''
                    }
                     post {
                         always {
                             junit 'jest-results/junit.xml'
                         }
                      }
                   }
                 stage ('E2E') {
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                        npm install serve
                        node_modules/.bin/serve -s build &
                        sleep 10
                        npx playwright test --reporter=html
                        '''
                    }
                }
                stage ('Deploy') {
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                        npm install netlify-cli
                        node_modules/.bin/netlify --version
                        echo "Deploying to Netlify with SiteID: $NETLIFY_SITE_ID"
                        '''
                    }
                   }
            }
        }
 }
   
}
