pipeline {
	    agent any
	

	        // Environment Variables
	        environment {
	        MAJOR = '1'
	        MINOR = '2'
	        //Orchestrator Services
	        UIPATH_ORCH_URL = "https://cloud.uipath.com/"
	        UIPATH_ORCH_LOGICAL_NAME = "jskharay"
	        UIPATH_ORCH_TENANT_NAME = "DefaultTenant"
	        UIPATH_ORCH_FOLDER_NAME = "My Workspace"
			//UIPATH_ORCH_ENVIORNMENT_NAME = ""
			//UIPATH_ORCH_ENVIORNMENT_NAME = "6392CCC8-9957-43A8-8899-B376B9D51A94"
			//UIPATH_ORCH_API_KEY = "lOF6W-A9pcaPb4nBB4rdmcZmqpNvsdp6Us9npOCh1Tqzb"
			UIPATH_ORCH_API_KEY = "NjnzyLoMeTsnxro9q9Ui5hDlyOck4tzkUpYKH-ypa4LUM"
			//env.BRANCH_NAME = "master"
	    }
	

	    stages {
	

	        // Printing Basic Information
	        stage('Preparing'){
	            steps {
	                echo "Jenkins Home ${env.JENKINS_HOME}"
	                echo "Jenkins URL ${env.JENKINS_URL}"
	                echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
	                echo "Jenkins JOB Name ${env.JOB_NAME}"
	                echo "GitHub BranhName ${env.BRANCH_NAME}"
	                checkout scm
	

	            }
	        }
	

	         // Build Stages
	        stage('Build') {
	            steps {
	                echo "Building..with ${WORKSPACE}"
	                UiPathPack (
	                      outputPath: "Output\\${env.BUILD_NUMBER}",
	                      projectJsonPath: "project.json",
	                      version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
	                      useOrchestrator: false,
			traceLevel: 'Verbose',
	        )
	            }
	        }
	         // Test Stages
	        stage('Test') {
	            steps {
	                echo 'Testing..the workflow...'
	            }
	        }
	

	         // Deploy Stages
	        stage('Deploy to UAT') {
	            steps {
	                echo "Deploying ${BRANCH_NAME} to UAT "
	                UiPathDeploy (
					packagePath: "Output\\${env.BUILD_NUMBER}",
	                orchestratorAddress: "${UIPATH_ORCH_URL}",
	                orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
	                folderName: "${UIPATH_ORCH_FOLDER_NAME}",
	                //environments: "",
	                //credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'APIUserKey']
	                //credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: "APIUserKey"), 
					credentials: Token(accountName: 'hHRylQckqPXCbBP07jLYRGFe_3WcjtYb1CKSDsP3Ul7ed', credentialsId: 'APIUserKey'),
					entryPointPaths: 'Main.xaml',
					traceLevel: 'Verbose',

					)
	            }
	        }
	

	

	         // Deploy to Production Step
	        stage('Deploy to Production') {
	            steps {
	                echo 'Deploy to Production'
	                }
	            }
	    }
	

	    // Options
	    options {
	        // Timeout for pipeline
	        timeout(time:80, unit:'MINUTES')
	        skipDefaultCheckout()
	    }
	

	

	    // 
	    post {
	        success {
	            echo 'Deployment has been completed!'
	        }
	        failure {
	          echo "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JOB_DISPLAY_URL})"
	        }
	        always {
	            /* Clean workspace if success */
	            cleanWs()
	        }
	    }
	

	}
