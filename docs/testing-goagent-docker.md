Testing goagent-docker
======================

Step-by-step instructions
-------------------------

### Start Go.CD Server

```
$ git clone https://github.com/gmacario/easy-genivigo
$ cd easy-genivigo
$ ./runme.sh
```

Browse `${GOCD_URL}` (example: http://192.168.99.100:8153) to access the Go.CD dashboard.

### Create pipeline `test_docker`

Browse `${GOCD_URL}`, then click **Admin** > **Pipelines**.

* Click **Create a new pipeline**

Step 1: Basic Settings

* Pipeline Name: `test_docker`
* Pipeline Group Name: `defaultGroup`

Click **NEXT**.

Step 2: Materials

* Material Type: Git
* URL: https://github.com/gmacario/easy-genivigo
* Branch: master
* Poll for new changes: Yes

Click **NEXT**.

Step 3: Stage/Job

* Configuration type: Define Stages

  - Stage Name: `defaultStage`
  - Trigger Type: On Success


* Initial Job and Task

  - Job Name: `defaultJob`
  - Task Type: More...
    - Command: `docker`
      
    - Arguments:
    
      ```
      --version
      ```
      
  - Working Directory: (none)

then click **FINISH**.

* Select pipeline `test_docker` > `defaultStage` > `defaultJob`

  - Add new task > More...

    - Command: `docker`

    - Arguments:

      ```
      info
      ```

    - Working Directory: (none)

then click **SAVE**.

* Select pipeline `test_docker` > `defaultStage` > `defaultJob`

  - Add new task > More...

    - Command: `docker-compose`

    - Arguments:

      ```
      --version
      ```

    - Working Directory: (none)

then click **SAVE**.

### Run pipeline `test_docker`

Browse `${GOCD_URL}`, then start pipeline `test_docker`.

Wait until pipeline `test_docker` is executed, then review the job console log

```
09:09:32.515 [go] Job Started: 2016-03-23 09:09:32 UTC

09:09:32.516 [go] Start to prepare test_docker/1/defaultStage/1/defaultJob on 613f1c30d3c9 [/var/lib/go-agent]
09:09:32.548 [go] Start to update materials.

09:09:32.549 [go] Start updating files at revision fb91c7f73b5f0e387dfa997494b11e341ca053cb from https://github.com/gmacario/easy-genivigo
09:09:32.555 STDERR: Cloning into '/var/lib/go-agent/pipelines/test_docker'...
09:09:34.507 [GIT] Fetch and reset in working directory pipelines/test_docker
09:09:34.508 [GIT] Cleaning all unversioned files in working copy
09:09:34.620 [GIT] Cleaning submodule configurations in .git/config
09:09:34.624 [GIT] Fetching changes
09:09:35.396 [GIT] Performing git gc
09:09:35.400 [GIT] Updating working copy to revision fb91c7f73b5f0e387dfa997494b11e341ca053cb
09:09:35.406 HEAD is now at fb91c7f Update README.md
09:09:35.507 [GIT] Removing modified files in submodules
09:09:35.544 [GIT] Cleaning all unversioned files in working copy
09:09:35.552 [go] Done.

09:09:35.556 [go] setting environment variable 'GO_SERVER_URL' to value 'https://goserver:8154/go/'
09:09:35.556 [go] setting environment variable 'GO_TRIGGER_USER' to value 'anonymous'
09:09:35.557 [go] setting environment variable 'GO_PIPELINE_NAME' to value 'test_docker'
09:09:35.557 [go] setting environment variable 'GO_PIPELINE_COUNTER' to value '1'
09:09:35.557 [go] setting environment variable 'GO_PIPELINE_LABEL' to value '1'
09:09:35.557 [go] setting environment variable 'GO_STAGE_NAME' to value 'defaultStage'
09:09:35.557 [go] setting environment variable 'GO_STAGE_COUNTER' to value '1'
09:09:35.557 [go] setting environment variable 'GO_JOB_NAME' to value 'defaultJob'
09:09:35.558 [go] setting environment variable 'GO_REVISION' to value 'fb91c7f73b5f0e387dfa997494b11e341ca053cb'
09:09:35.558 [go] setting environment variable 'GO_TO_REVISION' to value 'fb91c7f73b5f0e387dfa997494b11e341ca053cb'
09:09:35.558 [go] setting environment variable 'GO_FROM_REVISION' to value 'fb91c7f73b5f0e387dfa997494b11e341ca053cb'

09:09:35.599 [go] Start to build test_docker/1/defaultStage/1/defaultJob on 613f1c30d3c9 [/var/lib/go-agent]
09:09:35.600 [go] Current job status: passed.

09:09:35.600 [go] Start to execute task: <exec command="docker" >
<arg>--version</arg>
</exec>.
09:09:35.672 Docker version 1.10.3, build 20f81dd
09:09:35.675 [go] Current job status: passed.

09:09:35.675 [go] Start to execute task: <exec command="docker" >
<arg>info</arg>
</exec>.
09:09:36.093 Containers: 4
09:09:36.094  Running: 4
09:09:36.094  Paused: 0
09:09:36.096 WARNING: No swap limit support
09:09:36.097  Stopped: 0
09:09:36.098 Images: 150
09:09:36.099 Server Version: 1.10.3
09:09:36.099 Storage Driver: aufs
09:09:36.100  Root Dir: /var/lib/docker/aufs
09:09:36.101  Backing Filesystem: extfs
09:09:36.101  Dirs: 557
09:09:36.102  Dirperm1 Supported: true
09:09:36.102 Execution Driver: native-0.2
09:09:36.103 Logging Driver: json-file
09:09:36.103 Plugins: 
09:09:36.104  Volume: local
09:09:36.105  Network: host bridge null
09:09:36.105 Kernel Version: 3.16.0-67-generic
09:09:36.106 Operating System: Ubuntu 14.04.4 LTS
09:09:36.106 OSType: linux
09:09:36.107 Architecture: x86_64
09:09:36.108 CPUs: 2
09:09:36.108 Total Memory: 3.86 GiB
09:09:36.109 Name: ies-genbld01-vm
09:09:36.109 ID: CPUG:FEZF:RP3O:KQNB:X6HC:HBAO:OPLG:CO3L:2FNE:TYD4:FDBK:A6HX
09:09:36.110 Labels:
09:09:36.111  provider=generic
09:09:36.197 [go] Current job status: passed.

09:09:36.198 [go] Start to execute task: <exec command="docker-compose" >
<arg>--version</arg>
</exec>.
09:09:36.937 docker-compose version 1.6.2, build 4d72027
09:09:37.016 [go] Current job status: passed.

09:09:37.042 [go] Start to create properties test_docker/1/defaultStage/1/defaultJob on 613f1c30d3c9 [/var/lib/go-agent]
09:09:37.043 [go] Start to upload test_docker/1/defaultStage/1/defaultJob on 613f1c30d3c9 [/var/lib/go-agent]
09:09:37.268 [go] Job completed test_docker/1/defaultStage/1/defaultJob on 613f1c30d3c9 [/var/lib/go-agent]
```

<!-- EOF -->
