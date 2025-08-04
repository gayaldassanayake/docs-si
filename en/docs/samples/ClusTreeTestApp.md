# Performing Streaming Learning Using a Clustree Model

## Purpose

This sample demonstrates how to perform unsupervised streaming learning on a set of data points using a Clustree model.

!!!info "Before you begin:"
    1. Download `siddhi-gpl-execution-streamingml-x.x.x.jar` from [here](http://maven.wso2.org/nexus/content/repositories/wso2gpl/org/wso2/extension/siddhi/gpl/execution/streamingml/siddhi-gpl-execution-streamingml/) and place it in the `<SI_HOME>/lib` directory.<br/>
    2. Copy the `https://github.com/wso2/product-streaming-integrator-tooling/blob/master/modules/samples/artifacts/ClusTreeSample/clusTreeTestFeed.json` file and place it in the `<SI_HOME>/wso2/server/deployment/simulation-configs` directory.<br/>
    3. Copy the `https://github.com/wso2/product-streaming-integrator-tooling/blob/master/modules/samples/artifacts/ClusTreeSample/clusTreeFileTest.csv` file and place it in the `<SI_HOME>/wso2/server/deployment/csv-files` directory.<br/>
    4. Save the sample Siddhi application in WSO2 Integrator: SI Tooling.<br/>


## Executing the Sample

To execute the sample open the saved Siddhi application in WSO2 Integrator: SI Tooling, and start it by clicking the **Start** button (shown below) or by clicking **Run** => **Run**.

![Start button]({{base_path}}/images/amazon-s3-sink-sample/start.png)

If the Siddhi application starts successfully, the following message appears in the console.

`ClusTreeTestApp.siddhi - Started Successfully!`

## Testing the Sample

To test the sample Siddhi application, simulate multiple events via CSV file in the WSO2 Integrator: SI Tooling as follows:

1. To open the Event Simulator, click the **Event Simulator** icon.

    ![Event Simulator Icon]({{base_path}}/images/Testing-Siddhi-Applications/Event_Simulation_Icon.png)

    This opens the event simulation panel.



1. Open event simulator by clicking on the second icon or press Ctrl+Shift+I.
2. In the Feed Simulation tab of the panel you can see that the clusTreeTestFeed.csv file is loaded.
3. Press the play button to start simulation.

## Viewing the Results

After clicking the play button see the output on the console, that are produced according to the simulation from csv file.

???info "Click here to view the sample Siddhi application."

    ```sql
    @App:name("ClusTreeTestApp")


    define stream InputStream (x double, y double);

    @sink(type='log')
    define stream logStream (closestCentroidCoordinate1 double,closestCentroidCoordinate2 double,x double, y double);

    @info(name = 'query1')
    from InputStream#streamingml:clusTree(2, 10, 20, 5, 50, x, y)
    select closestCentroidCoordinate1, closestCentroidCoordinate2, x, y
    insert into logStream;
    ```

