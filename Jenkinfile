pipeline {
    agent any
    stages {
	stage('Build'){
	    steps{
		sh 'mvn clean package'
	    }
	    post{
		success{
		    echo 'now archiving...'
		    archiveArtifacts artifacts: '**/target/*.war'
		}
	    }
	}
	stage('deploy to staging'){
	    steps{
		build job: 'deploy to staging'
	    }
	}
	stage('deploy to production'){
	    steps{
		timeout(time:5, unit:'DAYS'){
		    input message:'Approve PRODUCTION deployment?'
		}
		build job: 'deploy_to_prod'
	    }
	    post{
		success{
		    echo 'code deployed to production'
		}
		failure{
		    echo 'deployment failed'
		}
	    }
	}
    }
}
