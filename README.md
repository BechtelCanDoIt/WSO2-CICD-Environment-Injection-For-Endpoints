This is a demo of one way to dynamicaly inject environment specific
endpoint urls into your WSO2 EI/MI code. This would be executed in 
the CI/CD process flow to migrate code from Dev -> Test -> UAT -> Prod. 

Steps:
1. Open WSO2 Integration Studio and create your endpoint using the wizard.
2. Create a project (or clone this one) structured like this one.
3. Copy in the endpoint.xml file and change the address to be dynamic
IE: :some-name-to-substitue
4. Create the conf files for your environments.
5. Delete your studio project if you no longer need it.

Example mvn command:
mvn clean install "-Denv=PROD"

Result:
<code>
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------< wso2.sampleHelloWorld:Demo-Endpoints >----------------
[INFO] Building Demo-Endpoints 1.0.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ Demo-Endpoints ---
[INFO] Deleting /Users/scottbechtel/dev/clients/support/CS0221318/EndpointInjectionDemo/Demo-Endpoints/target
[INFO]
[INFO] --- maven-resources-plugin:3.3.0:copy-resources (copy-resources-endpoints) @ Demo-Endpoints ---
[INFO] Copying 1 resource
[INFO]
[INFO] --- maven-resources-plugin:3.3.0:copy-resources (copy-resources-local-entries) @ Demo-Endpoints ---
...
[INFO] Installing /Users/scottbechtel/dev/clients/support/CS0221318/EndpointInjectionDemo/Demo-Endpoints/target/Demo-Endpoints-1.0.0-SNAPSHOT-TEST-bundle.zip to /Users/scottbechtel/.m2/repository/wso2/sampleHelloWorld/Demo-Endpoints/1.0.0-SNAPSHOT/Demo-Endpoints-1.0.0-SNAPSHOT-bundle.zip
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  3.414 s
[INFO] Finished at: 2023-02-08T11:11:40-06:00
[INFO] ------------------------------------------------------------------------
</code>

Example Injected Result from prod.conf file:
<code>
\<?xml version="1.0" encoding="UTF-8"?\>
\<endpoint name="DemoEP" xmlns="http://ws.apache.org/ns/synapse"\>
   \<http uri-template="http://PROD.company.com"\>
        \<suspendOnFailure\>
           \<initialDuration\>-1\</initialDuration\>
            \<progressionFactor\>1.0\</progressionFactor\>
        \</suspendOnFailure\>
        \<markForSuspension\>
            \<errorCodes\>-1\</errorCodes\>
            \<retriesBeforeSuspension\>0\</retriesBeforeSuspension\>
        \</markForSuspension\>
    \</http\>
\</endpoint\>
</code>
