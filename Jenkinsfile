pipeline {
  agent any

  tools {
    maven 'M2_HOME'
    }
  
  stages {
    stage('CheckOut') {
      steps {
        echo 'Checkout the source code from GitHub'
        git branch: 'master', url: 'https://github.com/sdwattamwar/Insure-Me.git'
            }
    }
   stage('Package') {
      steps {
        echo 'Generate a jar file the package using Maven'
        sh 'mvn clean package'
            }
    }
   stage('Publish TestNG report') {
      steps {
        echo 'Generate a TestNG report'
        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/Insure-Me/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
            }
    }
   stage('Create Docker Image') {
      steps {
        echo 'Create a Docker Image'
	sh 'docker --version'
        sh 'docker build -t sdwattamwar/insureme:1.0 .'
      	}
    }
     stage('Docker Login') {
      steps {
        echo 'Docker Login'
	withCredentials([usernamePassword(credentialsId: 'DockerHubCredentials', passwordVariable: 'DockerHub_PWD', usernameVariable: 'DockerHub_UN')]) {
		//echo 'userName:${DockerHub_UN}'
		//echo 'userName:${DockerHub_PWD}'
    		sh 'docker login -u ${DockerHub_UN} -p ${DockerHub_PWD}'
		}
             }
    }
     stage('Push Docker Image') {
      steps {
        echo 'Push a Docker Image'
        sh 'docker push sdwattamwar/insureme:1.0'
                   }
            }
    }
}

