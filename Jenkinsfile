pipeline {
    agent any

    stages {
        /*
        stage('Build') {
                agent{
                    docker{
                        image 'node:18-alpine'
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
        }*/
        stage('Test') {
                environment{
                    FILE_PATH = "build/index.html"
                }
                agent{
                    docker{
                        image 'node:18-alpine'
                        reuseNode true
                    }
                }
                steps {
                        /*sh '''
                        if [ -f "$FILE_PATH" ]; then
                            echo "Success: The file exists."
                        else
                            echo "Error: The file is missing."
                            exit 1
                        fi}
                        
                        '''*/
                        sh'npm test'
                }
    }
        stage('E2E') {
                agent{
                    docker{
                        image 'mcr.microsoft.com/playwright:v1.63.0-noble'
                        reuseNode true
                    }
                }
                steps {
                        sh '''
                        npm install -g serve
                        npm_modules/.bin/serve -s build &
                        sleep 10
                        npx playwright test
                        '''
                }
    }
}
    post{
        always{
            junit 'jest-results/juint.xml'
        }
    }

}