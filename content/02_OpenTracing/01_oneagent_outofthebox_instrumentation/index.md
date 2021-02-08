## OneAgent out-of-the-box instrumentation
In this step we will compile our Java Springboot application and explore the out-of-the-box instrumentation of the OneAgent

### Step 1: Compile and run the application
- Edit `hazelcast-client.yaml` in VHOT2021 > 02_OpenTracing > src\main > resources
  ![EditFile](../../../assets/images/01_oneagent_outofthebox_instrumentation-01.png)
- Input the public IP address given to you on the Zoom chat.
- Save the file, example `CTRL+S`
- Open up a Terminal from the top menu bar
  ![EditFile](../../../assets/images/01_oneagent_outofthebox_instrumentation-02.png)
- Change diretory to `02_OpenTracing` and run this command `mvn spring-boot:run`
  ```powershell
  PS C:\Users\dtu.training\Documents\vhot2021> cd 02_OpenTracing
  PS C:\Users\dtu.training\Documents\vhot2021\02_OpenTracing> mvn spring-boot:run
  ```
- Check that the program compiles without any errors and it connects to the HazelCast nodes
- Open a new terminal tab
  ![EditFile](../../../assets/images/01_oneagent_outofthebox_instrumentation-03.png)
- Enter `curl.exe http://localhost:8080/get?key=testkey`
- You will observe a response of `{"response":null}`. This is normal as you have not populated the HazelCast map with entries.
  ```powershell
  PS C:\Users\dtu.training\Documents\vhot2021> curl.exe http://localhost:8080/get?key=testkey
  {"response":null}
  ```
### Step 2: Populate the HazelCast instance with entries and retrieve the data
- Enter `curl.exe http://localhost:8080/pop1`
  ```powershell
  PS C:\Users\dtu.training\Documents\vhot2021> curl.exe http://localhost:8080/pop1
  {"response":"100 entry inserted to the map with key: 1.2.3.4-* , starting with 1 "}
  ```
- Retrieve the data with the key format as `<your public ip>-1`, example:
  ```powershell
  PS C:\Users\dtu.training\Documents\vhot2021> curl.exe http://localhost:8080/get?key=1.2.3.4-1
  {"response":1.2.3.4-1}
  ```
- Execute a few more `get` transactions with different keys (you can use the any number from 1 to 99). If you use a number outside of the range, you will get a response of `null`
- Once done, go back to the first terminal tab and terminate the Java program by using `CTRL+C`

  ![EditFile](../../../assets/images/01_oneagent_outofthebox_instrumentation-04.png)
  ```powershell
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD SUCCESS
  [INFO] ------------------------------------------------------------------------
  [INFO] Total time:  17:53 min
  [INFO] Finished at: 
  [INFO] ------------------------------------------------------------------------
  Terminate batch job (Y/N)?
  ```
  Enter Y

### Step 3: Explore Dynatrace PurePaths after auto instrumentation 
- Access your Dynatrace tenant in the browser
- Go to Transactions and Services > CommandController > Purepaths
  ![OpenTracingPurePaths](../../../assets/images/02-ExploreDynatrace-1.gif)
- Choose the /pop1 transaction
  ![OpenTracingPurePaths](../../../assets/images/02-ExploreDynatrace-2.gif)
- You will observe that the totoal transaction response time was 3 secs, but all of the Java method calls were less than 30 ms.
  ![OpenTracingPurePaths](../../../assets/images/02-ExploreDynatrace-3spp.png)
- Clearly something else is responsible
