pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID = '98848a0f-3977-4bbe-bd87-f20c8426e538'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('build') {
             agent{
                 docker{
                    image 'node:22'
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

            agent{
                docker{
                    image 'node:22'
                    reuseNode true
                }
            }

            steps{
                sh'''
                   test -f build/index.html || { echo "Build failed: index.html not found"; exit 1; }
                  npm test
                '''
            }
        }
        stage('deploy') {
             agent{
                 docker{
                    image 'node:22'
                    reuseNode true
                 }
             }

            steps {
                sh'''
                  
                  npm install netlify-cli --save-dev || { echo "Netlify CLI installation failed"; exit 1; }
                  npx netlify --version
                  npx netlify status
                  npx netlify deploy --dir=build --prod || { echo "Deployment failed"; exit 1; }
                  

                '''
            }
        }
    }
}
