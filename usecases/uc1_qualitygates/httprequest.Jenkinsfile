            println  "About to execute keptn evaluation"
            //Get the start time of the test
            def startTime =  get_timestamp()
            
            //Executute the Gatling tests
            maven tool: env.MAVEN_TOOL, commands: env.MAVEN_CMD, arguments: "-Dgatling.dataFolder=data  -Dgatling.simulationClass=${env.SIMULATION_CLASS} -Dtenant=${env.TENANT} -Dusers=${env.USER_COUNT} -Dduration=${env.DURATION} -DmaxDuration=${env.MAX_DURATION} -Dpacing=${env.PACING}"
            
            //Get the End Time of the Tests
            def endTime = get_timestamp()
            
            //Archive the Gatling Test Rsults
            archive()  
           
            //Define the Keptn Json payload for the evaluation request post
            def keptn_json = """
            {
             "type": "sh.keptn.event.start-evaluation",
             "source": "Jenkins",
             "data": {
                
               "start": "${startTime}Z",
                "end": "${endTime}Z",
                "project": "v1-application",
                "stage": "performance",
                "service": "vitality-home-screen-points-agreement-service-1-1",
                "teststrategy": "manual",
                "labels": {"environment": "${env.dynatrace_env}"}
              }
              }
             """
            // "labels": {"environment": "yourenv","yourney": "youryourney"}
             
            //Post the start-evaluation event to Keptn API
            def kept_context_request = httpRequest contentType: 'APPLICATION_JSON', customHeaders: [[maskValue: true, name: 'x-token', value: "${env.KEPTN_API_TOKEN}"]], httpMode: 'POST', requestBody: keptn_json, responseHandle: 'STRING', url: "${env.KEPTN_ENDPOINT}/v1/event", validResponseCodes: "100:404"
            
            //Parse response String 
            con_response = new JsonSlurperClassic().parseText( kept_context_request.getContent() )
        
            // Get the keptnContext Key
            keptn_context_key = con_response.get("keptnContext")
           
           // URL Encode the context key
            encoded_context = URLEncoder.encode(keptn_context_key.toString().trim(), "UTF-8")
           
           //Create variable for The keptn Status Request
            def kept_status_request
            
            //Query the Keptn API every 20 Seconds to ge the evaluation Done Event.
            timeout(time: 3, unit: 'MINUTES') {
                    script {
                        waitUntil {
                             sleep 20
                             // Post the Keptn Context to the Keptn api to get the Evaluation-done event
                             kept_status_request = httpRequest acceptType: 'APPLICATION_JSON', contentType: 'APPLICATION_JSON', customHeaders: [[maskValue: true, name: 'x-token', value: "${env.KEPTN_API_TOKEN}"]], httpMode: 'GET', responseHandle: 'STRING', url: "${env.KEPTN_ENDPOINT}/v1/event?keptnContext=${encoded_context}&type=sh.keptn.events.evaluation-done", validResponseCodes: "100:500"
                              
                              //The API returns a response code 500 error if the evalution done event does not exisit
                              if (kept_status_request.getStatus() == 500 || kept_status_request.getContent().toString().contains("No Keptn sh.keptn.events.evaluation-done event found for context") ) {
                                return false
                            } else {
                                return true
                            } 
                  }

               }
            }
            //println(kept_status_request.getContent())
            //Parse response String
            result_response = new JsonSlurperClassic().parseText( kept_status_request.getContent() )
            
            //Get the Result of the Evaluation
            keptn_result = result_response.get("data").get("result")
            
            //if The Keptn result is "Fail", fail the build.
            if (keptn_result == "fail") {
            error("Keptn_eval:${keptn_result}")
            }
def get_timestamp(){
    DATE_TAG = java.time.LocalDate.now()
    DATETIME_TAG = java.time.LocalDateTime.now()
    //echo "${DATETIME_TAG}
                
  return DATETIME_TAG
     