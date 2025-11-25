node {
    def appDir = '/var/www/nextjs-app'

    stage('Clean Workspace') {
        echo 'Cleaning workspace'
        deleteDir()
    }

    stage('Clone Repo') {
        echo 'Cloning the Repo'
        git(
            branch: 'main',
            url: 'https://github.com/Irfan-Haider05-spec/CICD-Jenkins-AWS.git'
        )
    }

    stage('Deploy to EC2') {
        echo 'Deploying to EC2'
        sh """
            # Create app directory
            sudo mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}

            # Sync files
            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}

            cd ${appDir}

            # Install deps
            npm install

            # Build project
            npm run build

            # Kill previous Next.js app on port 3000
            sudo fuser -k 3000/tcp || true

            # Start app in background
            nohup npm start > app.log 2>&1 &
        """
    }
}
