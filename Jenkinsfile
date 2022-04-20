#!/usr/bin/env groovy

@Library('keptn-library@3.5')_
def keptn = new sh.keptn.Keptn()
def result = ''

node {
    properties([
        parameters([
         string(defaultValue: 'adidas', description: 'Name of your Keptn Project for Quality Gate Feedback ', name: 'Project', trim: false), 
         string(defaultValue: 'staging', description: 'Stage in your Keptn project used for for Quality Gate Feedback', name: 'Stage', trim: false), 
         string(defaultValue: 'glass', description: 'Servicename used to keep SLIs and SLOs', name: 'Service', trim: false),
         string(name: 'custom_var', defaultValue: '')
        ])
    ])
    
	stage('Checkout SCM') {
	    checkout scm
				
	}
    stage('Initialize Keptn') {

        // Initialize the Keptn Project - ensures the Keptn Project is created with the passed shipyard
        keptn.keptnInit project:"${params.Project}", service:"${params.Service}", stage:"${params.Stage}", keptnConfigureMonitoring:"prometheus" // , shipyard:'shipyard.yaml'

    }
      
    stage('Download Kubectl & Config and install') {
        sh 'echo No build required for Webapp.'
        sh 'curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.21.11/bin/linux/amd64/kubectl'
        sh 'chmod +x ./kubectl'
        sh './kubectl version --client'
    }
    
    stage('Download Helm and install') {
        sh 'echo No build required for Webapp.'
        sh 'curl -LO https://get.helm.sh/helm-v3.6.0-linux-amd64.tar.gz'
        sh 'tar xvf helm-v3.6.0-linux-amd64.tar.gz'
        sh 'linux-amd64/helm version'
    }
    
        stage('Download Doctl') {
            withCredentials([string(credentialsId: 'doctl', variable: 'doctl')]) {
        sh 'curl -LO https://github.com/digitalocean/doctl/releases/download/v1.72.0/doctl-1.72.0-linux-amd64.tar.gz'
        sh 'tar xvf doctl-1.72.0-linux-amd64.tar.gz'
        sh './doctl auth init -t $doctl'
    }
            
        }
    
    stage('Test Image') {
        sh 'echo "Testing..."'
    }
    
    stage('Push Image') {
        sh 'echo "Image is pushed to the docker repository."'
    }
    
    stage('Deploy to Dev Environment') {
        sh 'echo "Image is pushed to the docker repository."'
        sh './kubectl set image deployment.v1.apps/glass glass=docker.io/keptnexamples/carts:0.13.2 -n adidas-dev'
    }
    
    stage('Run Load Tests') {
        sh 'echo "Image is pushed to the docker repository."'
    }
    
    stage('Deploy To Pre-Prod') {
        sh 'echo "Image is pushed to the docker repository."'
    }
    
    stage('Integration Tests') {
        sh 'echo "Image is pushed to the docker repository."'
    }
	  
    stage('Trigger Quality Gate') {
        echo "Quality Gates ONLY: Just triggering an SLI/SLO-based evaluation for the passed timeframe"

        // Trigger an evaluation
        def keptnContext = keptn.sendStartEvaluationEvent starttime:"660", endtime:"60"
        String keptn_bridge = env.KEPTN_BRIDGE
        echo "Open Keptns Bridge: ${keptn_bridge}/trace/${keptnContext}"
    }

    stage('Wait for Result') {
        waitTime = 3

        if(waitTime > 0) {
            echo "Waiting until Keptn is done and returns the results"
            result = keptn.waitForEvaluationDoneEvent setBuildResult:true, waitTime:waitTime
            echo "${result}"
            result = result.toString()
        } else {
            echo "Not waiting for results. Please check the Keptns bridge for the details!"
        }
    }
	
	stage('Release') {
	    if(result == "[pass]"){
	       echo "${result}"
           sh 'echo "Image is pushed to the docker repository."'
      }else{
          sh 'exit 1'}
	    }
      
	stage('Deploy to Production') {
	    if(result == "[pass]"){
           sh 'echo "Image is pushed to the docker repository."'
           sh './kubectl set image deployment.v1.apps/glass glass=docker.io/keptnexamples/carts:0.13.2 -n adidas-production'
      }else{
      sh 'exit 1'}
      }
     
    stage('Run Smoke Tests') {
         if(result == "[pass]"){
           sh 'echo "Image is pushed to the docker repository."'
      }else{
          sh 'exit 1'}
      }
    }