pipeline {
  agent any
  stages {
    stage('Build and start test image') {
      steps {
        sh 'docker-compose build'
        sh 'docker-compose up -d'
      }
    }
    stage('Run tests') {
      steps {
        sh '''docker-compose exec web python manage.py jenkins
'''
      }
    }
    stage('error') {
      steps {
        echo 'finished!'
      }
    }
  }
  environment {
    TEST_PREFIX = 'test-IMAGE'
    TEST_IMAGE = '${env.TEST_PREFIX}:${env.BUILD_NUMBER}'
    TEST_CONTAINER = '${env.TEST_PREFIX}-${env.BUILD_NUMBER}'
    COMPOSE_FILE = 'docker-compose.yml'
  }
}