## Prepare the environment
In this step we will prepare the environment for the hands on exercises
- Install OneAgent
- Enable OpenTelemetry instrumentation
- Clone the repos for the Java and Go exercise

### Step 1: Install OneAgent
- Access the Dynatrace tenant provided to you
- In your RDP Windows instance provided to you, download the Windows-based OneAgent from Deploy Dynatrace menu
- Start the installation with default settings

### Step 2: Enable OpenTelemetry and OpenTracing instrumentation
- Go to your environment, under `Settings` > `Server-side service monitoring` > `Deep Monitoring` > `OpenTelemetry and OpenTracing`
- You might also be required to go to `New OneAgent features` to enable some flags
  ![Deep Monitoring](../../assets/images/01-DeepMonitoringSettings.gif)

### Step 3: Clone the repos
- Launch Visual Studio Code
- Click on `clone repository`, and copy/enter

  `https://github.com/Dynatrace-APAC/vhot2021.git`

  ![CloneRepo](../../assets/images/01_prepare_the_environment-3.png)
- Select the folder (e.g. Documents folder) to store the cloned repo 
  ![Folder](../../assets/images/01_prepare_the_environment-4.png)
- If prompted to open the cloned repository, click on `Open`

  ![Open](../../assets/images/01_prepare_the_environment-5.png)

### You've arrived
- You are now ready to start the hands on!
  ![Ready](../../assets/images/01_prepare_the_environment-6.png)
