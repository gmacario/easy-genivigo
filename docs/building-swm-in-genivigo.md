Building GENIVI SWM PoC in easy-genivigo
========================================

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
  - Initial Job and Task
    - Job Name: `commonApiGenerationJob`
    - Task Type: More...
    - Properties: 
      - Command: `bash`
      - Arguments: 
      
      ```
      -c
      printenv && true
      ```
      
      - Working Directory: (none)

Click **FINISH**.

* Stages > Add new stage

  - Stage Name: `compileCppStage`
  - Trigger Type: On Success
  - Initial Job and Task
    - Job Name: `compileCppJob`
    - Task Type: More...
    - Properties: 
      - Command: `bash`
      - Arguments: 
      
      ```
      -c
      true
      ```
      
      - Working Directory: (none)

then click **SAVE**.

* Stages > Add new stage

  - Stage Name: `runSWLMPocStage`
  - Trigger Type: On Success
  - Initial Job and Task
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

Review pipeline `software-loading-manager` to make sure all the steps are as described above.

### Run pipeline `software-loading-manager`

Browse `${GOCD_URL}`, then click **PIPELINES**.

* Unpause pipeline `software-loading-manager`, then click the "Trigger" button.

Wait until pipeline `software-loading-manager` finishes, then review build log.

**FIXME**: The pipeline fails in `runSWLMPocStage` - see https://at.projects.genivi.org/jira/browse/TOOL-23.

```
09:33:41.817 [go] Job Started: 2016-05-24 09:33:41 UTC

09:33:41.817 [go] Start to prepare software-loading-manager/1/runSWLMPocStage/1/runSWLMPocJob on ca921f3ae51a [/var/lib/go-agent]
09:33:41.834 [go] Start to update materials.

09:33:41.834 [go] Start updating files at revision 0bda6ecfbd7ae7b7a03497895d3c2db51010c404 from https://github.com/GENIVI/genivi_swm
09:33:42.040 [GIT] Fetch and reset in working directory pipelines/software-loading-manager
09:33:42.040 [GIT] Cleaning all unversioned files in working copy
09:33:42.246 [GIT] Cleaning submodule configurations in .git/config
09:33:42.249 [GIT] Fetching changes
09:33:42.981 [GIT] Performing git gc
09:33:42.984 [GIT] Updating working copy to revision 0bda6ecfbd7ae7b7a03497895d3c2db51010c404
09:33:42.990 HEAD is now at 0bda6ec Package Manager Real Operations
09:33:43.090 [GIT] Removing modified files in submodules
09:33:43.116 [GIT] Cleaning all unversioned files in working copy
09:33:43.223 [go] Done.

09:33:43.224 [go] setting environment variable 'GO_SERVER_URL' to value 'https://goserver:8154/go/'
09:33:43.224 [go] setting environment variable 'GO_TRIGGER_USER' to value 'changes'
09:33:43.224 [go] setting environment variable 'GO_PIPELINE_NAME' to value 'software-loading-manager'
09:33:43.224 [go] setting environment variable 'GO_PIPELINE_COUNTER' to value '1'
09:33:43.225 [go] setting environment variable 'GO_PIPELINE_LABEL' to value '1'
09:33:43.225 [go] setting environment variable 'GO_STAGE_NAME' to value 'runSWLMPocStage'
09:33:43.225 [go] setting environment variable 'GO_STAGE_COUNTER' to value '1'
09:33:43.225 [go] setting environment variable 'GO_JOB_NAME' to value 'runSWLMPocJob'
09:33:43.225 [go] setting environment variable 'GO_REVISION' to value '0bda6ecfbd7ae7b7a03497895d3c2db51010c404'
09:33:43.225 [go] setting environment variable 'GO_TO_REVISION' to value '0bda6ecfbd7ae7b7a03497895d3c2db51010c404'
09:33:43.225 [go] setting environment variable 'GO_FROM_REVISION' to value '0bda6ecfbd7ae7b7a03497895d3c2db51010c404'

09:33:43.259 [go] Start to build software-loading-manager/1/runSWLMPocStage/1/runSWLMPocJob on ca921f3ae51a [/var/lib/go-agent]
09:33:43.259 [go] Current job status: passed.

09:33:43.259 [go] Start to execute task: <exec command="sudo" >
<arg>sh start_swm.sh -r</arg>
</exec>.
09:33:43.400 sudo: no tty present and no askpass program specified
09:33:43.442 [go] Current job status: failed.

09:33:43.458 [go] Start to create properties software-loading-manager/1/runSWLMPocStage/1/runSWLMPocJob on ca921f3ae51a [/var/lib/go-agent]
09:33:43.458 [go] Start to upload software-loading-manager/1/runSWLMPocStage/1/runSWLMPocJob on ca921f3ae51a [/var/lib/go-agent]
09:33:43.611 [go] Job completed software-loading-manager/1/runSWLMPocStage/1/runSWLMPocJob on ca921f3ae51a [/var/lib/go-agent]
```

<!-- EOF -->
