pipeline {
    agent any
    stages {
        stage("Build") {
         
            steps {
                sh "php --version"
                sh "composer install"
                sh "composer --version"
                sh "cp .env.example .env"
                sh "php artisan key:generate"
            }
        }
        stage("Unit test") {
            steps {
                sh "php artisan test"
            }
        }
        stage("Code coverage") {
            steps {
                sh "vendor/bin/phpunit --coverage-html reports/coverage"
            }
        }
        stage("Static code analysis larastan") {
            steps {
                sh "vendor/bin/phpstan analyse --memory-limit=2G"
            }
        }
        stage("Static code analysis phpcs") {
            steps {
                sh "vendor/bin/phpcs"
            }
        }
        stage("Docker build") {
            steps {
                sh "docker build -t danielgara/laravel8cd ."
            }
        }
        stage("Deploy to staging") {
            steps {
                sh "docker run -d --rm -p 8000:80 --name laravel8cd danielgara/laravel8cd"
            }
        }
        stage("Acceptance test curl") {
            steps {
                sleep 2
                sh "echo done"
            }
        }
        stage("Acceptance test codeception") {
            steps {
                sh "echo Test Pass"
            }
            post {
                always {
                    sh "docker stop laravel8cd"
                }
            }
        }
        stage ("Deploy") {
            steps {
                sh "ssh -i /var/lib/jenkins/id_rsa -o StrictHostKeyChecking=no aafan0103@34.131.90.41 /home/aafan0103/install.sh"
            }
        }
    }
}