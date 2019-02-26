pipeline {
  agent any
  stages {
    stage('Build and start test image') {
      steps {
        sh '''docker-composer build
docker-compose up -d
docker run --rm \\
    -v \'${env.WORKSPACE}\':\'/project\':ro \\
    -v /var/run/docker.sock:/var/run/docker.sock:ro \\
    -e TIMEOUT=30 \\
    -e COMPOSE_PROJECT_NAME=\\$(basename \\"\'${env.WORKSPACE}\'\\") \\
    softonic/compose-project-is-up

'''
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