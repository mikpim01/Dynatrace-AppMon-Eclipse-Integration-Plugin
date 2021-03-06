# FAQ / Troubleshooting Guide



## Problems? Questions? Suggestions?

Post any problems, questions or suggestions to the Dynatrace Community's [Application Monitoring & UEM Forum](https://answers.dynatrace.com/spaces/146/index.html).
 

## General

##### Dynatrace AppMon version compatibility - which version works with the plugin?
> 6.1 and newer

##### What is CodeLink?
> The ability to navigate from Dynatrace AppMon Client to code. E.g. this is available in the PurePaths view, from the PurePath Tree section: after right-clicking on a message call, selecting Source Lookup / Open in IDE. 

![img](/img/use/source_lookup.png)

## Configuration Help

##### Where is the AppMon Server configuration?
> In the Eclipse global preferences, under Dynatrace AppMon ( Window / Preferences / Dynatrace AppMon )

##### Where is the AppMon Agent configuration?
> Under the "Run with AppMon" [toolbar](/img/use/run_icon_on_bar.png): select "Run with AppMon Configurations...", select a relevant configuration, [Dynatrace AppMon tab](/img/conf/run_with_appmon_configuration_2.png)

##### Can I have the Dynatrace Agent connect directly into the AppMon Server without a collector?
> Yes, provided that you enable the Embedded Collector in AppMon Server configuration. To do that, run Dynatrace AppMon Client, open Settings menu \ Dynatrace Server... \ Services \ General tab \ Embedded Collector Settings section and select the "Allow Agent Connections to Dynatrace Server" option. Now, use the server's host and embedded collector's port as the collector's host and port into Eclipse global Dynatrace AppMon configuration under "Window \ Preferences \ Dynatrace AppMon".

##### Dynatrace AppMon Server port numbers -- where can I find them?
> Run Dynatrace AppMon Client, open Settings menu \ Dynatrace Server... \ Services \ Management tab \ Web Server section

##### Dynatrace AppMon server-as-collector port number -- where can I find it?
> Run Dynatrace AppMon Client, open Settings menu \ Dynatrace Server... \ Services \ General tab \ Embedded Collector Settings section. The default is 9998.

##### Can I configure multiple servers/collectors?
> No. There is a single server and collector configuration which you would have to change every time.
> But you can configure multiple profiles and agent names, one per launch configuration.

##### Where can I enable per-launch session recording?
> Configured per launch configuration, under ["Run with AppMon" toolbar](/img/use/run_icon_on_bar.png): select "Run with AppMon Configurations...", select a relevant configuration, [Dynatrace AppMon tab](/img/conf/run_with_appmon_configuration_2.png)

## Runtime problems

##### JUnit executions don't appear in AppMon.
> 1. Execute the launch configuration from the "Run with AppMon" icon !["Run with AppMon" icon](/img/use/run_icon_on_bar.png) vs the standard debug and run icons.
> 2. Check the Eclipse console for the agent output, confirm that it is able to connect to the collector. There is no popup or error message if this fails.
> 3. Check the system profile and agent name configured in the Dynatrace launch configuration. Confirm AppMon Server contains this profile, and the agent name is mapped to this profile ( Run with AppMon icon / Run with AppMon Configurations... \ Dynatrace AppMon tab )
> 3. Check the Eclipse Error Log view (Ctrl + 3, input "Error Log" in the popup) for errors
> 4. Confirm that you input (1) the dynatrace agent library (2) AppMon server, collector, client addresses into eclipse configuration under "Window \ Preferences \ Dynatrace AppMon"
> 5. Confirm that Eclipse has REST http/https connectivity to the server, collector and client.
> 6. Browse the REST requests and responses in the Eclipse Error Log view. There should be a POST request  to /rest/management/profiles/Monitoring/testruns, a 201 response with the testrunId (e.g. 47ca24ed-46df-425b-8b90-f3922f89bd18); afterwards polling requests to /rest/management/profiles/Monitoring/testruns/testrunId, with XML responses.

##### My Run configuration is not visible under "Run With AppMon Configurations"
> Only specific run configurations are supported. The currenlty supported run configurations are: Java application, JUnit test, Equinox OSGi application, Eclipse workbench application.

## Error messages (Eclipse Error Log)

##### Dynatrace Server configured to require SSL if authorization data provided - login failed, failed recording session [ErrorLocation-66]
> The server may be configured to accept authorization data only over secure (SSL) channel. You can configure the client to use the HTTP port (8021 by default) in the Eclipse preferences under Dynatrace AppMon, or reconfigure the server from the AppMon Client under Settings \ Dynatrace Server... \ Services \ Management Tab \ Accept authentication data only with HTTPS

##### Failed connecting to Dynatrace AppMon Client to poll for CodeLink jump requests. [ErrorLocation-79]
> Dynatrace AppMon Client is either not running or the Eclipse the client configuration is wrong (Window \ Preferences \ Dynatrace AppMon \ CodeLink ). To prevent the message from appearing please disable CodeLink temporarily and reenable as needed (Window Eclipse menu \ Preferences \ Dynatrace AppMon \ CodeLink \ Enable CodeLink )

##### No test results available at test retrieval timeout. Please verify agent and profile settings. [ErrorLocation-94]
> Partial results presented - full results not available within testrun retrieval timeout [ErrorLocation-95]
> The plugin polls the AppMon Server repeatadly waiting for the purePaths to be analized. Test results are processed on the server after a delay and don't arrive all at once. After 30 seconds the available results are presented. After 30 seconds the only way to view the test results is to open the AppMon Client.

##### Dynatrace Server is already recording a session, per-launch session couldn't be started [ErrorLocation-59]
> There is already a session started for the profile on the AppMon Server, or the Server is in continuous transaction storage mode. There can be only one session recorded per server profile. You can disable continuous transaction storage it in AppMon Client, Settings \ Dynatrace Server... \ Storage \ Storage Settings section \ Enable continuous transaction storage

## Usage hints

##### Easy way to create Dynatrace configurations
> Non-dynatrace Eclipse runtime configurations are visible by the plugin. Create one as you normally would, open the "Run With AppMon" \ Run With AppMon Configurations...", select the new configuration, add the Dynatrace AppMon config.

##### Browse REST request used by the plugin
> REST requests are written to the Eclipse log and visible in the Error Log view.

##### AppMon configuration: enable the Java Tests sensor pack placed on the eclipse agent tier 

otherwise no PurePaths (and test data) will be captured for your tests, see https://answers.dynatrace.com/questions/165503/eclipse-plugin-no-test-results-available.html
