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
				sh 'cd main; mvn -B -DskipTests clean package'
            }
			post {
				always {
					recordIssues enabledForFailure: true, tools: [mavenConsole(), java(), javaDoc()]
				}
			}
        }
	stage('Quality Test') {
		steps {
			sh 'cd main; mvn pmd:pmd pmd:cpd spotbugs:potbugs checkstyle:checkstyle'
		}
		post {
			always {
			    recordIssues enabledForFailure: true, tool: checkStyle()
			    recordIssues enabledForFailure: true, tool: spotBugs()
			    recordIssues enabledForFailure: true, tool: cpd(pattern: '**/target/cpd.xml')
			    recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
			}
    	}
	}
    stage('Unit Test') {
        steps {
			sh 'cd main; mvn test'
			jacoco( 
				  execPattern: '**/*.exec',
			)
        }
        post {
            always {
                junit 'main/commons/target/surefire-reports/*.xml'
                junit 'main/keychain/target/surefire-reports/*.xml'
                junit 'main/launcher/target/surefire-reports/*.xml'
                junit 'main/ui/target/surefire-reports/*.xml'
            }
        }
    }
    stage('Deliver') {
        steps {
            sh 'cd main; mvn install -Prelease'
        }
    }
}
}
