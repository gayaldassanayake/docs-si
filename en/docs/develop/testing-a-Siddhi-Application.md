# Running and Testing Siddhi Applications

The WSO2 Integrator: SI allows the following tasks to be carried
out to ensure that the Siddhi applications you create and deploy are
validated before they are run in an actual production environment.

-   Validate Siddhi applications that are written using the WSO2 Integrator: SI VSCode extension.
-   Run Siddhi applications that were written using the WSO2 Integrator: SI VSCode extension.
-   Simulate events to test the Siddhi applications and analyze events
    that are received and sent. This allows you to analyze the status of
    each query within a Siddhi application at different execution
    points.

## Validating a Siddhi application

To validate a Siddhi application, follow the procedure below:

1.  Open the VSCode within the workspace that contains the Siddhi application.

2.  In this example, we are using the **ReceiveAndCount** sample from [Siddhi Application Overview](./siddhi-Application-Overview.md). Copy the entire Siddhi application code
    and paste it in a new file. Save the file with a `.siddhi` extension.
    ```sql
        @App:name("ReceiveAndCount")
    @App:description('Receive events via HTTP transport and view the output on the console')

    /* 
        Sample Siddhi App block comment
    */

    -- Sample Siddhi App line comment

    @Source(type = 'http',
            receiver.url='http://localhost:8006/productionStream',
            basic.auth.enabled='false',
            @map(type='json'))
    define stream SweetProductionStream (name string, amount double);

    @sink(type='log')
    define stream TotalCountStream (totalCount long);

    -- Count the incoming events
    @info(name='query1')
    from SweetProductionStream
    select count() as totalCount
    insert into TotalCountStream;
    ```

3.  This sample does not have errors, and
    therefore, no errors are displayed in the editor. To create an error
    for demonstration purposes, change the `count()` function in the `query1` query to
    `countNew()` as shown below.

    ```sql
    @info(name='query1') 
    from SweetProductionStream 
    select countNew() as totalCount 
    insert into TotalCountStream;      `
    ```

    Now, the editor indicates that there is a syntax error. If you move
    the cursor over the error icon, it indicates that
    `countNew` is an invalid function name as shown
    below.
    ![Source View indicates a syntax error]({{base_path}}/images/Testing-Siddhi-Applications/Syntax_Error_Indicated.png)

## Running a Siddhi application

You can run a Siddhi application to verify whether the logic
you have written is correct. To start a Siddhi application, follow the procedure below:

1.  Open the application in VSCode.

2.  In the editor, click the **Run** button (▶️) in the toolbar.

    ![Run button]({{base_path}}/images/Testing-Siddhi-Applications/Run-Button.png)

<a name="simulate"></a>
## Simulating events

This section demonstrates how to test a Siddhi application via event simulation. Event simulation involves simulating predefined event streams. These event stream definitions have stream attributes. You can use event simulator to create events by assigning values to the defined stream attributes and send them as events. This is useful for testing Siddhi applications in order to evaluate whether they function as expected.


Events can be simulated in the following methods:

- [Simulating a single event](#Simulating-a-single-event)
- [Simulating multiple events via CSV files](#Simulating-multiple-events-via-CSV-files)
- [Simulating multiple events via databases](#Simulating-multiple-events-via-databases)
- [Generating random events](#Generating-random-events)

!!! tip
    Before you simulate events for a Siddhi application, you need to run it. Therefore, before you try this section, see [Running a Siddhi application](testing-a-Siddhi-Application.md#running-a-siddhi-application).

### Simulating a single event

This section demonstrates how to simulate a single event to be processed
via the WSO2 Integrator: SI.

To simulate a single event, follow the steps given below.

1.  Run the Siddhi application from the VSCode editor. It will pop up the **WSO2 Integrator: SI Event Simulator** panel.
    
2.  Click the **Single Simulation** panel.
    It opens the left panel for event simulation as follows.

    <img src="{{base_path}}/images/Testing-Siddhi-Applications/Single-Event-Simulation-Panel.png" alt="Single Simulation Panel" width="50%">
    <!-- ![Event Simulation Panel]({{base_path}}/images/Testing-Siddhi-Applications/Single-Event-Simulation-Panel.png) -->

3.  Enter Information in the **Single Simulation** panel as described
     below.

    1.  In the **Siddhi App Name** field, select a currently deployed Siddhi application.

    2.  In the **Stream Name** field, select the event stream for which you want to simulate events. The list displays all the event streams defined in the selected Siddhi application.

    3.  If you want to simulate the event for a specific time different to the current time, enter a valid timestamp in the **Timestamp** field. To select a timestamp, click the time and calendar icon next to the **Timestamp** field.

        ![Time and Calendar icon]({{base_path}}/images/Testing-Siddhi-Applications/Time_And_Calendar.png) <br/>

        Then select the required date, hour, minute, second and millisecond. Click **Done** to select the time stamp entered. If you want to select the current time, you can click **Now**.

    4.  Enter values for the attributes of the selected stream.

4.  Click **Send** to start sending the event. The simulated event is logged similar to the sample log given below.

    ![Output Log]({{base_path}}/images/Testing-Siddhi-Applications/Output_Log.png)


### Simulating multiple events via CSV files
This section explains how to generate multiple events via CSV files to
be analyzed via the WSO2 Integrator: SI.

!!! tip
    Before simulating events, a Siddhi application should be deployed.
    
To simulate multiple events from a CSV file, follow the steps given
below.

1.  Run the Siddhi application from the VSCode editor. It will pop up the **WSO2 Integrator: SI Event Simulator** panel.
    
2.  Click the **Feed Simulation** panel.
    It opens the left panel for event simulation as follows.
    <img src="{{base_path}}/images/Testing-Siddhi-Applications/Feed-Simulation-Panel.png" alt="Feed Simulation Panel" width="50%">

3.  To create a new simulation, click **Create** . This opens the
    following panel.
    <img src="{{base_path}}/images/Testing-Siddhi-Applications/Feed-Simulation.png" alt="New feed simulation" width="50%">

4.  Enter values for the displayed fields as follows.

    1.  In the **Simulation Name** field, enter a name for the event simulation.

    2.  In the **Description** field, enter a description for the event simulation.

    3.  If you want to receive events only during a specific time interval, enter that time interval in the **Time Interval** field.

    4.  Click **Advanced Configurations** if you want to enter detailed specifications to filter events from the CSV file. Then enter information as follows.

        1.  If you want to include only events that belong to a specific time interval in the simulation feed, enter the start time and the end time  in the **Starting Event's Timestamp** and **Ending Event's Timestamp** fields respectively. To select a timestamp, click the time and calendar icon next to the field.

            ![Time and calendar icon]({{base_path}}/images/Testing-Siddhi-Applications/Time_And_Calendar.png)

            Then select the required date, hour, minute, second and millisecond. Click **Done** to select the time stamp entered. If you want to select the current time, you can click **Now**.

        2.  If you want to restrict the event simulation feed to a specific number of events, enter the required number in the **No of Events** field.

    5.  In the **Simulation Source** field, select **CSV File**.

    6.  Click **Add Simulation Source** to open the following section.

        ![Simulation Source]({{base_path}}/images/Testing-Siddhi-Applications/Simulation_Source.png)

        In the **Siddhi App Name** field, select the required Siddhi
        application. Then more fields as shown below.

        ![Feed simulation details]({{base_path}}/images/Testing-Siddhi-Applications/Feed_Simulation_Details.png)

        Enter details as follows:

        1.  In the **Stream Name** field, select the stream for which you want to simulate events. All the streams defined in the Siddhi application you selected are available in the list.

        2.  In the **CSV File** field, select an available CSV file. If no CSV files are currently uploaded, select **Upload File** from the list. This opens the **Upload File** dialog box.

            ![Uploading the CSV file]({{base_path}}/images/Testing-Siddhi-Applications/Upload_CSV.png)

            Click **Choose File** and browse for the CSV file you want to upload. Then click **Upload**.

        3.  In the **Delimiter** field, enter the character you want to use in order to separate the attribute values in each row of the CSV file.

        4.  If you want to enter more detailed specifications, click **Advanced Configuration**. Then enter details as follows.

            1.  To use the index value as the event timestamp, select the **Timestamp Index** option. Then enter the relevant index.

            2.  If you want to increase the value of the timestamp for each new event, select the **Increment event time by(ms)** option. Then enter the number of milliseconds by which you want to increase the timestamp of each event.

            3.  If you want the events to arrive in order based on the timestamp, select **Yes** under the **Timestamp Interval** option.

        5.  Click **Save** to save the information relating to the CSV file. The name os the CSV file appears in the **Feed Simulation** tab in the left panel.

6.  To simulate a CSV file that is uploaded and visible in the **Feed Simulation** tab in the left panel, click on the arrow to its right. The simulated events are logged in the output console.

### Simulating multiple events via databases

This section explains how to generate multiple events via databases to be analyzed via the WSO2 Integrator: SI.

!!! tip
    Before simulating events via databases:<br/>
    -   A Siddhi application must be created.<br/>
    -   The database from which you want to simulate events must be already configured for the WSO2 Integrator: SI.
    
To simulate multiple events from a database, follow the procedure below:

1. Run the Siddhi application from the VSCode editor. It will pop up the **WSO2 Integrator: SI Event Simulator** panel.

2.  Click the **Feed Simulation** panel.
    It opens the left panel for event simulation as follows.
    <img src="{{base_path}}/images/Testing-Siddhi-Applications/Feed-Simulation-Panel.png" alt="Feed Simulation Panel" width="50%">

3.  To create a new simulation, click **Create** . This opens the
    following panel.
    <img src="{{base_path}}/images/Testing-Siddhi-Applications/Feed-Simulation.png" alt="New feed simulation" width="50%">

4.  Enter values for the displayed fields as follows.

    1.  In the **Simulation Name** field, enter a name for the event simulation.

    2.  In the **Description** field, enter a description for the event simulation.

    3.  If you want to simulate events at time intervals of a specific length, enter that length in milliseconds in the **Time Interval(ms)** field.

    4.  If you want to enter more advanced conditions to simulate the events, click **Advanced Configurations**. As a result, the following section is displayed.

        ![Advanced onfigurations]({{base_path}}/images/Testing-Siddhi-Applications/Feed_Simulation_Database_Advanced.png)

        Then enter details as follows:

        1.  If you want to include only events that belong to a specific time interval in the simulation feed, enter the start time and the end time  in the **Starting Event's Timestamp** and **Ending Event's Timestamp** fields respectively. To select a timestamp, click the time and calendar icon next to the field.

            ![Time and Calendar icon]({{base_path}}/images/Testing-Siddhi-Applications/Time_And_Calendar.png)

            Then select the required date, hour, minute, second and millisecond. Click **Done** to select the time stamp entered. If you want to select the current time, you can click **Now**.

        2.  If you want to restrict the event simulation feed to a specific number of events, enter the required number in the **No of Events** field.

    5.  In the **Simulation Source** field, select **Database**. To connect to a new database, click **Add Simulation Source** to open the following section.

        ![Configuring the database]({{base_path}}/images/Testing-Siddhi-Applications/Configure_Database_For_Simulation.png)

        Enter information as follows:

        | Field               | Description                                                                                                                                                 |
        |---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
        | **Siddhi App Name** | Select the Siddhi Application in which the event stream for which you want to simulate events is defined.                                                   |
        | **Stream Name**     | Select the event stream for which you want to simulate events. All the streams defined in the Siddhi Application you selected are available to be selected. |
        | **Data Source**     | The JDBC URL to be used to access the required database.                                                                                                    |
        | **Driver Class**    | The driver class name of the selected database.                                                                                                             |
        | **Username**        | The username that must be used to access the database.                                                                                                      |
        | **Password**        | The password that must be used to access the database.                                                                                                      |

    6.  Once you have entered the above information, click **Connect to Database**. If the datasource is correctly configured, the following is displayed to indicate that WSO2 Integrator: SI can successfully connect to the database.

        ![Successfully connected to database]({{base_path}}/images/Testing-Siddhi-Applications/Successfully_Connected_Message.png)

    7.  To use the index value as the event timestamp, select the **Timestamp Index** option. Then enter the relevant index. If you want the vents in the CSV file to be sorted based on the timestamp, select the **Yes** option under **CSV File is Ordered by Timestamp**.

    8.  To increase the timestamp of the published events, select the **Timestamp Interval** option. Then enter the number of milliseconds by which you want to increase the timestamp of each event.

6.  Click **Save**. This adds the fed simulation you created as an active simulation in the **Feed Simulation** tab of the left panel as shown below.

    ![Feed simulation added]({{base_path}}/images/Testing-Siddhi-Applications/Created_Feed-Simulation.png)

7.  Click on the play button of this simulation to open the **Start the Siddhi Application** dialog box.

    ![Start the Siddhi Application dialog box]({{base_path}}/images/Testing-Siddhi-Applications/start-siddhi-application.png)

8.  Click **Start Simulation**. A message appears to inform you that the feed simulation started successfully. Similarly, when the simulation is completed, a message appears to inform you that the event simulation has finished.

### Generating random events

This section explains how to generate random data to be analyzed via the WSO2 Integrator: SI.

!!! tip
    Before simulating events, a Siddhi application should be deployed.
    
To simulate random events, follow the steps given below:

1. Run the Siddhi application from the VSCode editor. It will pop up the **WSO2 Integrator: SI Event Simulator** panel.

2.  Click the **Feed Simulation** panel.
    It opens the left panel for event simulation as follows.
    <img src="{{base_path}}/images/Testing-Siddhi-Applications/Feed-Simulation-Panel.png" alt="Feed Simulation Panel" width="50%">

3.  To create a new simulation, click **Create** . This opens the
    following panel.
    <img src="{{base_path}}/images/Testing-Siddhi-Applications/Feed-Simulation.png" alt="New feed simulation" width="50%">

4.  Enter values for the displayed fields as follows.

    1.  In the **Simulation Name** field, enter a name for the event simulation.

    2.  In the **Description** field, enter a description for the event simulation.

    3.  If you want to include only events that belong to a specific time interval in the simulation feed, enter the start time and the end time  in the **Starting Event's Timestamp** and **Ending Event's Timestamp** fields respectively. To select a timestamp, click the time and calendar icon next to the field.

        ![Time and Calendar icon]({{base_path}}/images/Testing-Siddhi-Applications/Time_And_Calendar.png)

        Then select the required date, hour, minute, second and millisecond. Click **Done** to select the time stamp entered. If you want to select the current time, you can click **Now**.

    4.  If you want to restrict the event simulation feed to a specific number of events, enter the required number in the **No of Events** field.

    5.  If you want to receive events only during a specific time interval, enter that time interval in the **Time Interval** field.

    6.  In the **Simulation Source** field, select **Random**.

    7.  If the random simulation source from which you want to simulate events does not already exist in the **Feed Simulation** pane, click **Add New** to open the following section.

        ![Random Simulation Source]({{base_path}}/images/Testing-Siddhi-Applications/Random_Simulation_Source.png)

    8.  Enter information relating to the random source as follows:

        1.  In the **Siddhi App Name** field, s elect the name of the
            Siddhi App with the event stream for which the events are
            simulated.
        2.  In the **Stream Name** field, select the event stream for
            which you want to simulate events. All the streams defined
            in the Siddhi Application you selected are available to be
            selected.
        3.  In the **Timestamp Interval** field, enter the number of
            milliseconds by which you want to increase the timestamp of
            each event.
        4.  To enter values for the stream attributes, follow
            the instructions below.
            -   To enter a custom value for a stream attribute, select
                **Custom data based** from the list. When you select
                this value, data field in which the required value can
                be entered appears as shown in the example below.  
                ![Custom data based simulation]({{base_path}}/images/Testing-Siddhi-Applications/Custom-data-based.png){width="235"
                height="88"}
            -   To enter a primitive based value, select **Primitive
                based** from the list. The information to be entered
                varies depending on the data type of the attribute. The
                following table explains the information you need to
                enter when you select **Primitive based** for each data
                type.  

                <table>
                <thead>
                <tr class="header">
                <th>Data Type</th>
                <th>Values to enter</th>
                </tr>
                </thead>
                <tbody>
                <tr class="odd">
                <td><code>                     STRING                    </code></td>
                <td>Specify a length in the <strong>Length</strong> field that appears. This results in a value of the specified length being auto-generated.</td>
                </tr>
                <tr class="even">
                <td><code>                     FLOAT                    </code> or <code>                     DOUBLE                    </code></td>
                <td>The value generated for the attribute is based on the following values specified.
                <ul>
                <li><strong>Min</strong> : The minimum value.</li>
                <li><strong>Max</strong> : The maximum value.</li>
                <li><strong>Precision</strong> : The precise value. The number of decimals included in the auto-generated values are the same as that of the value specified here.</li>
                </ul></td>
                </tr>
                <tr class="odd">
                <td><code>                     INT                    </code> or <code>                     LONG                    </code></td>
                <td>The value generated for the attribute is based on the following values specified.
                <ul>
                <li><strong>Min</strong> : The minimum value.</li>
                <li><strong>Max</strong> : The maximum value.</li>
                </ul></td>
                </tr>
                <tr class="even">
                <td><code>                     BOOL                    </code></td>
                <td>No further information is required because <code>                     true                    </code> and <code>                     false                    </code> values are randomly generated.</td>
                </tr>
                </tbody>
                </table>

            -   To randomly assign values based on a pre-defined set of meaningful values, select **Property based** from the list. When you select this value, a field in which the set of available values are listed appears as shown in the example below.

                ![Property-based simulation]({{base_path}}/images/Testing-Siddhi-Applications/Property-based.png)

            -   To assign a regex value, select **Regex based** from the list.

6.  Click **Save** to save the simulation information. The saved random simulation appears in the **Feed** tab of the left panel.

7.  To simulate events, click the arrow to the right of the saved simulation (shown in the example below).

    ![Simulating random events]({{base_path}}/images/Testing-Siddhi-Applications/Simulate_Events.png)

    The simulated events are logged in the CLI as shown in the extract
    below.

    ![Random Events Output Log]({{base_path}}/images/Testing-Siddhi-Applications/Random_Events_Output_Log.png)  