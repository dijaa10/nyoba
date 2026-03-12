node {
    checkout scm

    stage("Build") {
        docker.image('rareloop/php-composer:php-8.2-cli-composer-2').inside('-u root') {
            sh 'composer install'
        }
    }

    stage("Test") {
        docker.image('ubuntu:22.04').inside('-u root') {
            sh 'echo "Ini adalah test"'
        }
    }
}