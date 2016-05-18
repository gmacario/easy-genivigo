Building GENIVI SWM PoC in easy-genivigo
========================================

**WORK-IN-PROGRESS**

Step-by-step instructions
-------------------------

### Start Go.CD Server

```
$ git clone https://github.com/gmacario/easy-genivigo
$ cd easy-genivigo
$ ./runme.sh
```

Browse `${GOCD_URL}` as shown in the console (example: http://192.168.99.100:8153) to access the Go.CD dashboard.

**NOTE**: On a clean installation it may take a few minutes before the Go.CD server begins serving requests from your web client. You may check the startup progress with the following command:

```
$ eval $(docker-machine env easy-genivigo)
$ docker-compose logs
```

### Create pipeline `software-loading-manager`

**TODO**

Browse `${GOCD_URL}`, then click **Admin** > **Pipelines**.

* Click **Create a new pipeline**

Step 1: Basic Settings

* Pipeline Name: `software-loading-manager`
* Pipeline Group Name: `defaultGroup`

Click **NEXT**.

Step 2: Materials

* Material Type: Git
* URL: `https://github.com/GENIVI/genivi_swm`
* Branch: master
* Poll for new changes: Yes

Click **NEXT**.

Step 3: Stage/Job

* Configuration type: Define Stages

  - Stage Name: `commonApiGenerationStage`
  - Trigger Type: On Success
    * Initial Job and Task
      - Job Name: `commonApiGenerationJob`
      - Task Type: More...
      - Properties: 
        - Command: `bash`
        - Arguments: `-c printenv && true`
        - Working Directory: (none)

Click **FINISH**.

* Stages > Add new stage

  - Stage Name: `compileCppStage`
  - Trigger Type: On Success
    * Initial Job and Task
      - Job Name: `compileCppJob`
      - Task Type: More...
      - Properties: 
        - Command: `bash`
        - Arguments: `true`
        - Working Directory: (none)

* Stages > Add new stage

  - Stage Name: `runSWLMPocStage`
  - Trigger Type: On Success

    * Initial Job and Task
      - Job Name: `runSWLMPocJob`
      - Task Type: More...
      - Command:
  
        ```
        sudo
        ```
  
      - Arguments:
  
        ```
        sh start_swm.sh -r
        ```
  
      - Working Directory: (none)

    then click **SAVE**.

Review pipeline `software-loading-manager`, then click **SAVE**.

### Run pipeline `software-loading-manager`

Browse `${GOCD_URL}`, then click **PIPELINES**.

* Start pipeline `software-loading-manager`.

Wait until pipeline `software-loading-manager` finishes, then review build log.

```
TODO
```

<!-- EOF -->