pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '5b82086f-aae7-4d5f-8e42-4aa39003da96'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('build') {
            agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        // stage('parallel test') {
        //     parallel {
        //         stage('test') {
        //             agent {
        //                 docker {
        //                     image 'node:20-alpine'
        //                     reuseNode true
        //                 }
        //             }
        //             steps {
        //                 sh '''
        //                     echo "Test Stage"
        //                     test -f build/index.html
        //                     npm test
        //                 '''
        //             }
        //             post () {
        //                 always {
        //                     junit 'jest-results/junit.xml'
        //                 }
        //             }
        //         }

        //         stage('e2e') {
        //             agent {
        //                 docker {
        //                     image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
        //                     reuseNode true
        //                 }
        //             }
        //             steps {
        //                 sh '''
        //                     npm install serve
        //                     node_modules/.bin/serve -s build &
        //                     sleep 5
        //                     npx playwright test --reporter=html
        //                 '''
        //             }
        //         }
        //     }
        // }

        stage('deploy') {
            agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploying to production. Site ID $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }
    }
}
