node {
    stage('Checkout') {
        checkout scm
    }
    stage('Build') {
        docker.image('rareloop/php-composer:php-8.2-cli-composer-2').inside('-u root') {
            sh '''
            git config --global --add safe.directory /var/jenkins_home/workspace/laravel-dev
            composer install --no-interaction --prefer-dist --optimize-autoloader
            '''
        }
    }
    stage('Test') {
        docker.image('ubuntu:22.04').inside('-u root') {
            sh '''
            echo "Ini adalah test"
            '''
        }
    }
    stage('Deploy') {
    sshagent (credentials: ['ssh-prod']) {
        sh '''
        apt-get update -qq && apt-get install -y rsync
        mkdir -p ~/.ssh
        chmod 700 ~/.ssh
        ssh-keyscan -H 172.20.209.222 >> ~/.ssh/known_hosts
        rsync -rav --delete \
        -e "ssh -o StrictHostKeyChecking=no" \
        ./ dj@172.20.209.222:/home/dj/prod.kelasdevops.xyz/ \
        --exclude=.env \
        --exclude=storage \
        --exclude=.git
        '''
    }
}
}