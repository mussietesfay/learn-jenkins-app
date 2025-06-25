pipeline {
    agent any

    stages {
        stage('build') {
             agent{
                 docker{
                    image 'node:22-alpine'
                    reuseNode true
                 }
             }

            steps {
                sh'''
                  ls -al
                  node --version
                  npm --version
                  npm ci
                  npm run build
                  ls -al
                '''
            }
        }
        stage('test'){

            steps{
                echo "test stage"
            }
        }
    }
}
