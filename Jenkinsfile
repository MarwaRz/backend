pipeline {
  agent any
  
  stages {
    stage('login to ecr') {
      when {
        branch "devops"
      }
      steps {
        echo 'logging in ...'
        sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 313220989398.dkr.ecr.us-east-1.amazonaws.com'
        }
      }
    

    stage('building docker image') {
      when {
        branch "devops"
      }
      steps {
        echo 'building ...'
        sh 'docker build -t devops/backend .'
      }
    }

    stage('tagging image') {
      when {
        branch "devops"
      }
      steps {
        echo 'tagging ...'
        sh 'docker tag devops/backend:latest 313220989398.dkr.ecr.us-east-1.amazonaws.com/devops/backend:latest'
      }
    }

    stage('pushing image') {
      when {
        branch "devops"
      }
      steps {
        echo 'pushing to registry ...'
        sh 'docker push 313220989398.dkr.ecr.us-east-1.amazonaws.com/devops/backend:latest'
      }
    }
  }
  post {
    always {
      echo 'This will always run' 
      echo 'Deploying devops...'
      sh 'docker compose --project-name devops up -d'
      echo 'devops Deployed'
    }
  }
}
