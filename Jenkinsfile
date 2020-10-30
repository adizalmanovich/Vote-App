pipeline {
  agent {
    node {
      stage('Checkout') {
      git url: 'https://github.com/adizalmanovich/Voting-App.git'
    }
  }	
  stages {
    stage('Build result') {
      steps {
	sh """
	cd result
        docker build -t docker-registry.local.com:5000/votingapp_result .
	cd ..
	"""
      }
    } 
    stage('Build vote') {
      steps {
        sh """
        cd vote
        docker build -t docker-registry.local.com:5000/votingapp_vote .
        cd ..
        """
      }
    }
    stage('Build worker') {
      steps {
        sh """
        cd worker
        docker build -t docker-registry.local.com:5000/votingapp_worker .
        cd ..
        """
      }
    }
    stage('Push result image') {
      when {
        branch 'master'
      }
      steps {
        withDockerRegistry(credentialsId: 'mydockeprivrcred', url: 'http://docker-registry.local.com:5000') {
          sh 'docker push docker-registry.local.com:5000/votingapp_result'
        }
      }
    }
    stage('Push vote image') {
      when {
        branch 'master'
      }
      steps {
        withDockerRegistry(credentialsId: 'mydockeprivrcred', url: 'http://docker-registry.local.com:5000') {
          sh 'docker push docker-registry.local.com:5000/votingapp_vote'
        }
      }
    }
    stage('Push worker image') {
      when {
        branch 'master'
      }
      steps {
        withDockerRegistry(credentialsId: 'mydockeprivrcred', url: 'http://docker-registry.local.com:5000') {
          sh 'docker push docker-registry.local.com:5000/votingapp_worker'
        }
      }
    }
  }
}
