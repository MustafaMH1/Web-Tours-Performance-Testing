
# 1. Why do we need to use CLI Mode ( NON-GUI-Mode)

- It’s always recommended to run JMeter non-GUI tests instead of GUI tests as you will avoid many performance related problems on JMeter.
- The reason is that the JMeter GUI is not designed to execute high loads, and consumes lots of resources that could overload the server, which would give you incorrect information about your test results. Additionally, the more resource consumption the server has, the less load you will be able to generate in your tests.
- Non-GUI mode is very powerful, almost every feature that could be used from JMeter GUI could be also used either through command line or the properties files.
- Therefore, knowing how to run JMeter non-GUI tests in an effective way is very important to ensure proper performance testing.

## 2. Using CLI Mode (Command Line mode was called NON GUI mode)

For load testing, you must run JMeter in this mode (Without the GUI) to get the optimal results from it. To do so, use the following command options:

- **`n`** This specifies JMeter is to run in cli mode
- **`t`** [name of JMX file that contains the Test Plan].
- **`l`** [name of JTL file to log sample results to].
- **`j`** [name of JMeter run log file].
- **`r`** Run the test in the servers specified by the JMeter property "**remote_hosts**"
- **`R`** [list of remote servers] Run the test in the specified remote servers
- **`g`** [path to CSV file] generate report dashboard only
- **`e`** generate report dashboard after load test
- **`o`** output folder where to generate the report dashboard after load test. Folder must not exist or be empty

The script also lets you specify the optional firewall/proxy server information:

- **`H`** [proxy server hostname or IP address]
- **`P`** [proxy server port]

**Example**

```
jmeter -n -t my_test.jmx -l log.jtl -H my.proxy.server -P 8000
```

# **3. Monitoring & Reporting**

- The main disadvantage of running in non-GUI mode is that you can’t see the real-time results on the listeners. However, there are some other options that enables the tester to see live results during the executions.
    1. Understand the information given by JMeter while running in non-GUI mode, which is not as much information as shown in the listeners but is useful in order to get a basic idea of the test results. 
    2.  **Generate an HTML report using JMeter in CLI mode**
        
        **Run the JMeter test** from the command line with the following options:
        
        ```
        jmeter -n -t <path to JMX file> -l <path to CSV result file> -e -o <path to output folder>
        ```
        
        - **`n`** specifies non-GUI mode
        - **`t`** is the path to the JMX test plan file
        - **`l`** is the path to the CSV file to log results to
        - **`e`** generates the report at the end of the test
        - **`o`** is the path to the output folder for the HTML report
        1. **The output folder** specified with **`o`** will contain the generated HTML report files after the test completes.
        2. **The HTML report** provides useful metrics and graphs, including:
            - APDEX table for transaction performance
            - Request summary graph showing success/failure percentages
            - Statistics table with summary metrics per transaction
            - Error table with error details
            - Top 5 Errors by Sampler
            - Charts like Response Times Over Time, Active Threads Over Time
    3. **Backend-Listener**
        - The backend listener is an Asynchronous listener that enables you to plug custom implementations of [BackendListenerClient](https://jmeter.apache.org/api/org/apache/jmeter/visualizers/backend/BackendListenerClient.html). Which Allows for real-time monitoring of the test
            
         ![Grafite Dashboard](https://github.com/MustafaMH1/Web-Tours-Performance-Testing/blob/main/Docs/Extras/Grafite%20Dashboard.png)
            
        
    4. **Running Test using [Taurus](https://gettaurus.org/)**
    5. **Using Grafana InfluxDB backend listener**
        1. **Set up the InfluxDB Backend Listener in JMeter** to send test results to InfluxDB:
            - Download the JMeter-InfluxDB-Writer plugin and add the JAR file to the **`/lib/ext`** directory of your JMeter installation.
            - In your JMeter test plan, add a Backend Listener and select **`JMeterInfluxDBBackendListenerClient`** as the implementation.
            - Configure the Backend Listener with your InfluxDB connection details and specify which samplers to record.
        2. **Create an InfluxDB data source in Grafana** and configure it to connect to your InfluxDB instance.
        3. **Import a pre-built JMeter dashboard** from the Grafana community:
            - Go to the Grafana Dashboards page and search for "JMeter".
            - Select a dashboard that suits your needs, such as the "JMeter Load Test" dashboard.
            - Click on the dashboard to view its details and click "Import" to add it to your Grafana instance.
        4. **Customize the dashboard** as needed:
            - Adjust the time range to view the desired test results.
            - Modify the panels and queries to display the specific metrics you want to analyze.
            - Add annotations to mark significant events during the test.
        5. **Monitor the test results in real-time** as the JMeter test runs. The dashboard will display metrics such as:
            - Response times
            - Throughput
            - Error rates
            - Active threads
            - Bytes transferred
        
        ![Grafana Dashboard](https://github.com/MustafaMH1/Web-Tours-Performance-Testing/blob/main/Docs/Extras/Grafana%20DashBoard.png)
        
    6. **Running Test Using BlazeMeter ( Online Service )**
    

# **Note :  Remove JMeter Listeners**

It’s a good practice to use as few listeners as possible, especially avoiding the ones that show real-time results, like “View Results Tree” as : 

1. **Listeners consume a significant amount of memory and CPU resources**. The more listeners you have, especially real-time ones like "View Results Tree", the more resources JMeter will use, which can impact the performance of your load test.
2. **Listeners like "View Results Tree" display detailed information about each sample result in real-time**. While useful for debugging in GUI mode, these listeners generate a lot of data, especially during high load tests, further increasing memory usage.

## Refrences
1. [Jmeter Documentation](https://jmeter.apache.org/usermanual/index.html)
2. [Blaze Meter](https://www.blazemeter.com/blog/jmeter-non-gui-mode)
3. [Grafana Documentation](https://grafana.com/docs/grafana/latest/)
4. [Taturas Documentation](https://gettaurus.org/docs/Index/)
