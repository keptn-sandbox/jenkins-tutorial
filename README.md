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

**Example 1: Integration via Jenkins httprequest plugin**
One way of doing this is shown in the sample [Jenkins Pipeline file](./usecases/uc1_qualitygates/httprequest.Jenkinsfile) that [Leon Van Zyl](https://github.com/leonvzGit) contributed to this tutorial.


## 2. Integrate Keptn's Performance Testing as a Self-Service in your Jenkins Pipeline

TBD

## 3. Keptn invokes Jenkins pipelines for Deployment 

TBD


## 4. Keptn invokes Jenkins pipelines for Testing

TBD
