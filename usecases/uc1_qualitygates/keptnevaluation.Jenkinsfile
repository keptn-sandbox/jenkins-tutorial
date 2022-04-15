@Library('keptn-library@3.5')_
def keptn = new sh.keptn.Keptn()

node {
    properties([
        parameters([
         string(defaultValue: 'adidas', description: 'Name of your Keptn Project for Quality Gate Feedback ', name: 'Project', trim: false), 
         string(defaultValue: 'staging', description: 'Stage in your Keptn project used for for Quality Gate Feedback', name: 'Stage', trim: false), 
         string(defaultValue: 'glass', description: 'Servicename used to keep SLIs and SLOs', name: 'Service', trim: false)
        ])
    ])

    stage('Initialize Keptn') {

        // Initialize the Keptn Project - ensures the Keptn Project is created with the passed shipyard
        keptn.keptnInit project:"${params.Project}", service:"${params.Service}", stage:"${params.Stage}", keptnConfigureMonitoring:"prometheus" // , shipyard:'shipyard.yaml'

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
            def result = keptn.waitForEvaluationDoneEvent setBuildResult:true, waitTime:waitTime
            echo "${result}"
        } else {
            echo "Not waiting for results. Please check the Keptns bridge for the details!"
        }
    }
}