pipeline {
	agent { label 'ansible' }
    environment { 
        devopsFolder = '.'
        ansibleFolder = '.'
    }
    options {
        timeout(time: 1, unit: 'HOURS') 
    }
    parameters {
        string(name: 'node', defaultValue: '', description: 'Node name')
        string(name: 'disk', defaultValue: '', description: 'Raw disk name')
        string(name: 'pvSize', defaultValue: '1', description: 'LV and PV size in GB')
        booleanParam(name: 'doPV', defaultValue: false, description: 'Mark if is needed to do persistent volume (PV) for the project')
        booleanParam(name: 'doBackup', defaultValue: false, description: 'Mark if is needed a daily doBackup of the content')
        booleanParam(name: 'doClaim',  defaultValue: false, description: 'Mark if is needed to do claim (PVC) for the project')
        string(name: 'inventory',  defaultValue: '', description: 'Ansible inventory')
        string(name: 'playbook',  defaultValue: './playbooks/openshift-create-pv-lv.yml', description: 'Ansible inventory')
        string(name: 'credendialsId',  defaultValue: '', description: 'Ansible credentials')
        string(name: 'openshiftUrl',  defaultValue: '', description: 'Openshift url')
        string(name: 'openshiftNameSpace',  defaultValue: '', description: 'Openshift project name')
        password(name: 'openshiftToken',  defaultValue: '', description: 'Openshift token')
        string(name: 'claimName', defaultValue: '', description: 'Name of the claim in the Openshift project')
    }
	stages {	
		stage('Ansible Playbook') {
		    steps {
		    	dir(ansibleFolder) {
	        		ansiblePlaybook(
			            playbook: devopsFolder + playbook,
					    credentialsId: credendialsId,
					    sudo: true,
			            inventory: inventory,
			            extraVars: [
			            	node: node,
			                size: pvSize,
			                disk: disk,
			                kube_nfs_volumes_kubernetes_url: openshiftUrl,
			                openshift_namespace: openshiftNameSpace,
			                claim_name: claimName,
			                do_PV: doPV,
			                backup: doBackup,
			                do_Claim: doClaim,
			                kubernetes_token: openshiftToken
			            ],
			            colorized: true)
				}
		    }
	    }
	}
	post {
		success {
		  echo "success"
		}
		failure {
			emailext (
			   subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
			   body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
			     <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
			   mimeType: 'text/html',
			   recipientProviders: [[$class: 'DevelopersRecipientProvider']]
			 )
		}
	}
}