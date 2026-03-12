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
        # 1. Pastikan folder .ssh ada (pakai flag -p supaya tidak error kalau sudah ada)
        mkdir -p ~/.ssh
        chmod 700 ~/.ssh

        # 2. Ambil fingerprint server tujuan supaya tidak muncul prompt (yes/no)
        ssh-keyscan -H 172.20.209.222 >> ~/.ssh/known_hosts
        
        # 3. Jalankan rsync (Hapus baris apt-get install)
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