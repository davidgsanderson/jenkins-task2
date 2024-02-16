pipeline {
  agent any
  stages {
    stage ('Clean-up') {
      steps {
        script {
          sh 'docker rm -f $(docker ps -a) || true'
          sh 'docker rmi $(docker images) || true'
          sh 'docker volume rm $(docker volume ls) || true'
          sh 'docker builder prune --all --force'
        }
      }
    }
    stage ('Build Images') {
      steps {
        script {
          sh 'docker build -t flask-app ./flask-app'
          sh 'docker build -t trio-mysql ./db'
        }
      }
    }
    stage ('Build Network') {
      steps {
        script {
         sh 'docker network create flask_app_network_2 || true'
        }
      }
    }
    stage ('Build Volume') {
      steps {
        script {
         sh 'docker volume create db-data || true'
        }
      }
    }
// 2 slashes to comment
    stage('Deploy MYSQL App'){
      steps{
        sh 'docker run -d --name trio-mysql --network flask_app_network_2 --volume db-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=password MYSQL_DATABASE=flask-db flask-app'
      }
    }
    stage('Deploy Flask App'){
      steps{
        sh 'docker run -d --name flask-app --network flask_app_network_2 -e YOUR_NAME=Andrew flask-app'
      }
    }
    stage('Deploy nginx App'){
      steps{
        sh 'docker run -d -p 80:80 --network flask_app_network_2 --name nginx --mount type=bind,source=$(pwd)/nginx.conf,target=/etc/nginx/nginx.conf nginx:alpine'
      }
    }
  }
}
