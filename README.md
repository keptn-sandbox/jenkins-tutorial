# Tutorials and Samples for Integrating Jenkins with Keptn

| Author | Keptn-Version |
| ------ | ------------- |
| grabnerandi | 0.6.1 |

The goal of this repository is to describe integration use cases of Keptn with Jenkins. As there are many integration scenarios and also different ways to integrate Keptn with Jenkins we also collect different approaches, scripts, plugins, ... that the community is building to support these use cases.

If you have built your own scripts or plugins please let us know. Feel free to add your custom scripts to the scripts subfolder by issuing a Pull Request (PR)

## Use Case Overview (work in progress)

| Use Case | Description |
| ------ | ------------- |
| Integrate Keptn's SLI/SLO-based Quality Gates in your existing Jenkins Pipeline | If your pipeline already deploys and runs tests you can invoke the Keptn Quality Gate by send a start-evaluation event, wait until evaluation-done event is available and use the result to fail or succeed your Jenkins Pipeline |
| Integrate Keptn's Performance Testing as a Self-Service in your Jenkins Pipeline | If your pipeline already deploys you can let Keptn automate test execution and Quality Gate Evaluation by sending a deployment-finished event from your Jenkins Pipeline |
| Keptn invokes Jenkins pipelines for Deployment | If you have Jenkins pipelines that deploys your application you can use Keptn to orchestrate the end-2-end continuos delivery process but invoking your Jenkins Pipeline to do the actual deployment of the artifact/app |
| Keptn invokes Jenkins pipelines for Test Execution | If you have Jenkins pipelines that tests your application you can use Keptn to orchestrate the end-2-end continuos delivery process but invoking your Jenkins Pipeline to do the actual testing of the artifact/app |

## 1. Integrate Keptn's SLI/SLO-based Quality Gates

This is a straight forward use case where your Jenkins Pipeline simply triggers an SLI/SLO-based Quality Gate Evaluation in Keptn. This can either be done through the Keptn CLI or the API. 

**Example 1: Integrate via Keptn Jenkins Shared Pipeline Library**
The Keptn Jenkins Shared Library provides a lot of helper functions to connect your Jenkins Pipeline with a Keptn Project and allows you to easily trigger the Keptn Quality Gate. All you need is
1. A Jenkins Server with the installed [Keptn Jenkins Shared Library](https://github.com/keptn-sandbox/keptn-jenkins-library). Make sure you follow all instructions on that GitHub page
2. Create a new Jenkins Pipeline and reference or copy [keptnevaluation.Jenkinsfile](./usecases/uc1_qualitygates/keptnevaluation.Jenkinsfile). You can call it "Keptn Quality Gate Evaluation"

This example comes with a pre-defined set of SLIs and SLOs for Dynatrace. If you want to use a different monitoring tool simply change the SLI.yaml to e.g: pull this data from Prometheus. If you want to just follow along with Dynatrace then make sure you have any type of application deployed and monitored by a Dynatrace OneAgent. If you don't have Dynatrace yet just sign up for the [Dynatrace SaaS trial](http://bit.ly/dtsaastrial).
My SLI.yaml uses tags to identify the service you want to pull your SLI data from. The name of the tag can be passed to our Jenkins Pipeline as a parameter. The default value is "evalservice" which means you only need to place a tag on your service you want to have evaluated with that name. Like shown in the following screenshot:
![](./images/evalservice_tag_dynatrace.png)

Now we are good to go to execute the Jenkins Pipeline to trigger our Quality Gates:
1. If this is the first time you execute the pipeline it will fail as Jenkins hasnt parsed the parameters yet and therefore runs into an error on a choice parameter
2. Once failed - now click on "Build with Parameters" and go with the following defaults:
![](./images/pipeline_evaluation_executewithparameters.png)



**Example 2: Integration via Jenkins httprequest plugin**
If you dont want to use the Jenkins Shared Library you can do it by calling the Keptn API directly in your pipeline!
One way of doing this is shown in the sample [Jenkins Pipeline file](./usecases/uc1_qualitygates/httprequest.Jenkinsfile) that [Leon Van Zyl](https://github.com/leonvzGit) contributed to this tutorial. Leon has shared a part of a Jenkins Pipeline that executes a Gatling test, sends a Keptn start-evaluation event for the timeframe of the test execution and then waits for the evaluation to be done!

## 2. Integrate Keptn's Performance Testing as a Self-Service in your Jenkins Pipeline

TBD

## 3. Keptn invokes Jenkins pipelines for Deployment 

TBD


## 4. Keptn invokes Jenkins pipelines for Testing

TBD
