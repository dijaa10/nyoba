node {
    checkout scm

    stage("Build") {
        docker.image('shippingdocker/php-composer:7.4').inside('-u root') {
            sh 'rm -f composer.lock'
            sh 'composer install'
        }
    }

    stage("Test") {
        docker.image('ubuntu:22.04').inside('-u root') {
            sh 'echo "Ini adalah test"'
        }
    }

    stage("PHP Check") {
        docker.image('shippingdocker/php-composer:7.4').inside('-u root') {
            sh 'php -v'
            sh 'composer -V'
        }
    }
}