@Library('keptn-library@3.5')_
def keptn = new sh.keptn.Keptn()

node {
    properties([
        parameters([
         string(defaultValue: 'adidas', description: 'Name of your Keptn Project you have setup for progressive delivery', name: 'Project', trim: false), 
         string(defaultValue: 'staging', description: 'First stage you want to deploy into', name: 'Stage', trim: false), 
         string(defaultValue: 'carts', description: 'Name of the service you provide a configuration change for', name: 'Service', trim: false),
         string(defaultValue: 'docker.io/keptnexamples/carts:0.13.1', description: 'Name of the service you provide a configuration change for', name: 'Image', trim: false),
        ])
    ])

    stage('Initialize Keptn') {
        // keptn.downloadFile("https://raw.githubusercontent.com/keptn-sandbox/performance-testing-as-selfservice-tutorial/master/shipyard.yaml", 'keptn/shipyard.yaml')
        // keptn.downloadFile("https://raw.githubusercontent.com/keptn-sandbox/jenkins-tutorial/master/usecases/uc1_qualitygates/keptn/dynatrace/dynatrace.conf.yaml", 'keptn/dynatrace/dynatrace.conf.yaml')
        // keptn.downloadFile("https://raw.githubusercontent.com/keptn-sandbox/jenkins-tutorial/master/usecases/uc1_qualitygates/keptn/slo_${params.SLI}.yaml", 'keptn/slo.yaml')
        // keptn.downloadFile("https://raw.githubusercontent.com/keptn-sandbox/jenkins-tutorial/master/usecases/uc1_qualitygates/keptn/dynatrace/sli_${params.SLI}.yaml", 'keptn/sli.yaml')
        archiveArtifacts artifacts:'keptn/**/*.*'

        // Initialize the Keptn Project - ensures the Keptn Project is created with the passed shipyard
        keptn.keptnInit project:"${params.Project}", service:"${params.Service}", stage:"${params.Stage}", keptnConfigureMonitoring:"prometheus" // , shipyard:'shipyard.yaml'

        // Upload all the files
        // keptn.keptnAddResources('keptn/dynatrace/dynatrace.conf.yaml','dynatrace/dynatrace.conf.yaml')
        // keptn.keptnAddResources('keptn/sli.yaml','dynatrace/sli.yaml')
        // keptn.keptnAddResources('keptn/slo.yaml','slo.yaml')
    }

    stage('Trigger Delivery') {
        echo "Progressive Delivery: Triggering Keptn to deliver ${params.Image}"

        // send deployment finished to trigger tests
        // def keptnContext = keptn.sendConfigurationChangedEvent project:"${params.Project}", service:"${params.Service}", stage:"${params.Stage}", image:"${params.Image}" 
        def keptnContext = keptn.sendDeliveryTriggeredEvent image:"${params.Image}"
        String keptn_bridge = env.KEPTN_BRIDGE
        echo "Open Keptns Bridge: ${keptn_bridge}/trace/${keptnContext}"
    }
    stage('Wait for Result') {
        waitTime = 60

        if(waitTime > 0) {
            echo "Waiting until Keptn is done and returns the results"
            def result = keptn.waitForEvaluationDoneEvent setBuildResult:true, waitTime:waitTime
            echo "${result}"
        } else {
            echo "Not waiting for results. Please check the Keptns bridge for the details!"
        }
    }
}