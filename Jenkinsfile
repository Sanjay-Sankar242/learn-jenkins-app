pipeline {
    agent any

    stages {
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
        }
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
                        sh '''
                        if [ -f "$FILE_PATH" ]; then
                            echo "Success: The file exists."
                        else
                            echo "Error: The file is missing."
                            exit 1
                        fi
                        npm test
                        '''
                }
    }
}
    post{
        always{
            junit 'test-results/juint.xml'
        }
    }

}