# KEPServerEX-MySQL-Connector

KEPServerEX itself does not support the direct transfer of data to a MySQL database. We write a Python script to import information from KEPServerEX into a MySQL database.

This Python script is primarily designed to periodically read data from a KEPServerEX OPC UA server and write this data into a MySQL database. The script can be broken down into several key sections:

1. **Import Necessary Libraries**:
   - `mysql.connector`: Used for connecting to and operating on a MySQL database.
   - `opcua`: Used for communicating with an OPC UA server (in this case, KEPServerEX).
   - `time`: Used to control the interval at which data is read.

2. **Configure KEPServerEX and MySQL Connection Information**:
   - `opc_url` defines the URL address of the KEPServerEX OPC UA server.
   - `mysql_config` contains the information needed to connect to the MySQL database (host, username, password, database name).

3. **Define the `write_to_mysql` Function**:
   - This function is responsible for writing the data read into the MySQL database. It attempts to connect to the database, execute the insert statement, and commit the changes. If an error occurs, it prints the error message.

4. **Define the `main` Function**:
   - Initially, it attempts to connect to the KEPServerEX OPC UA server.
   - Then, based on configured node IDs (`node_ids`), it retrieves the node objects from the KEPServerEX server (representing different data points, such as the motion axes of a robot and the status of the power switch).
   - It enters an infinite loop, periodically (once every second) reading the values of these nodes, printing the read data, and writing the data to the MySQL database by calling the `write_to_mysql` function.
   - If any exceptions are encountered during the process, error messages are printed, and the connection to the OPC UA server is disconnected at the end.

5. **Loop Execution and Data Reading**:
   - The script creates an infinite loop with `while True`, where it:
     - Reads data from specified nodes on the OPC UA server.
     - Prints the read data to the console.
     - Calls the `write_to_mysql` function to write the data into the MySQL database.
     - Pauses for 1 second with `time.sleep(1)` to achieve periodic reading.

6. **Exception Handling**:
   - The script catches and prints error messages if any exceptions occur while attempting to connect to the OPC UA server, read data, or write to the database, ensuring that the script does not fail silently in case of issues.

7. **Script Execution Entry Point**:
   - If this script is run directly (rather than being imported as a module), the `main` function will be executed.

