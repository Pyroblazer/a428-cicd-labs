node {
    docker.image('node:lts-buster-slim').inside {
        stage('Checkout') {
            checkout scm
        }
        
        stage('Build') {
            sh 'npm install'
        }
    
        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }
        
        stage('Approval') {
            input message: 'Finished using the website? (Click "Proceed" to continue)'
        }

        stage('Deploy') {
            steps {
                echo 'Installing Cloudflare Wrangler'
                sh 'yarn add wrangler'
                echo 'Deploying to cloudflare pages'
                sh 'CLOUDFLARE_ACCOUNT_ID=4e7ebdc8a1d44a8db238fe265919c51d CLOUDFLARE_API_TOKEN=5TxvEmqAuQEGxDgInw4BU0FaQF_XI26sPVKT6ISG npx wrangler pages publish build --project-name=a428-cicd-labs'
                sh './jenkins/scripts/deliver.sh'
                sh 'sleep 60'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
