pipeline {
    agent {
	any{
        	image 'maven:3-alpine'
        	args '-v /root/.m2:/root/.m2'
	}
    }
    stages {
        stage('Build') {
            steps {
		sh 'cd ./main'
                sh 'mvn -e -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
		sh 'cd main'
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
