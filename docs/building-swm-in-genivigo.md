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
      sed
      ```

    - Arguments:

      ```
      -i
      -e
      s/^SWM_SIMULATION\ \=.*$/SWM_SIMULATION = True/
      common/settings.py
      ```

    - Working Directory: (none)

then click **SAVE**.

* Tasks > Add new task
  - Task Type: More...
      - Command:
  
        ```
        bash
        ```
  
      - Arguments:
  
        ```
        -xe
        -c
        ls -la && ls -la common && cat common/settings.py
        ```
  
      - Working Directory: (none)

then click **SAVE**.

* Tasks > Add new task
  - Task Type: More...
      - Command:
  
        ```
        which
        ```
  
      - Arguments:
  
        ```
        python
        ```
  
      - Working Directory: (none)

then click **SAVE**.

* Tasks > Add new task
  - Task Type: More...
      - Command:
  
        ```
        bash
        ```
  
      - Arguments:
  
        ```
        -xe
        start_swm.sh
        -r
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
09:53:11.822 [go] Job Started: 2016-05-24 09:53:11 UTC

09:53:11.822 [go] Start to prepare software-loading-manager/5/runSWLMPocStage/1/runSWLMPocJob on ca921f3ae51a [/var/lib/go-agent]
09:53:11.834 [go] Start to update materials.

09:53:11.834 [go] Start updating files at revision 0bda6ecfbd7ae7b7a03497895d3c2db51010c404 from https://github.com/GENIVI/genivi_swm
09:53:11.940 [GIT] Fetch and reset in working directory pipelines/software-loading-manager
09:53:11.940 [GIT] Cleaning all unversioned files in working copy
09:53:12.047 [GIT] Cleaning submodule configurations in .git/config
09:53:12.149 [GIT] Fetching changes
09:53:12.862 [GIT] Performing git gc
09:53:12.965 [GIT] Updating working copy to revision 0bda6ecfbd7ae7b7a03497895d3c2db51010c404
09:53:12.970 HEAD is now at 0bda6ec Package Manager Real Operations
09:53:13.070 [GIT] Removing modified files in submodules
09:53:13.094 [GIT] Cleaning all unversioned files in working copy
09:53:13.299 [go] Done.

09:53:13.299 [go] setting environment variable 'GO_SERVER_URL' to value 'https://goserver:8154/go/'
09:53:13.299 [go] setting environment variable 'GO_TRIGGER_USER' to value 'changes'
09:53:13.299 [go] setting environment variable 'GO_PIPELINE_NAME' to value 'software-loading-manager'
09:53:13.299 [go] setting environment variable 'GO_PIPELINE_COUNTER' to value '5'
09:53:13.300 [go] setting environment variable 'GO_PIPELINE_LABEL' to value '5'
09:53:13.300 [go] setting environment variable 'GO_STAGE_NAME' to value 'runSWLMPocStage'
09:53:13.300 [go] setting environment variable 'GO_STAGE_COUNTER' to value '1'
09:53:13.300 [go] setting environment variable 'GO_JOB_NAME' to value 'runSWLMPocJob'
09:53:13.300 [go] setting environment variable 'GO_REVISION' to value '0bda6ecfbd7ae7b7a03497895d3c2db51010c404'
09:53:13.300 [go] setting environment variable 'GO_TO_REVISION' to value '0bda6ecfbd7ae7b7a03497895d3c2db51010c404'
09:53:13.300 [go] setting environment variable 'GO_FROM_REVISION' to value '0bda6ecfbd7ae7b7a03497895d3c2db51010c404'

09:53:13.337 [go] Start to build software-loading-manager/5/runSWLMPocStage/1/runSWLMPocJob on ca921f3ae51a [/var/lib/go-agent]
09:53:13.337 [go] Current job status: passed.

09:53:13.337 [go] Start to execute task: <exec command="sed" >
<arg>-i</arg>
<arg>-e</arg>
<arg>s/^SWM_SIMULATION\ \=.*$/SWM_SIMULATION = True/</arg>
<arg>common/settings.py</arg>
</exec>.
09:53:13.343 [go] Current job status: passed.

09:53:13.343 [go] Start to execute task: <exec command="bash" >
<arg>-xe</arg>
<arg>-c</arg>
<arg>ls -la && ls -la common && cat common/settings.py</arg>
</exec>.
09:53:13.346 + ls -la
09:53:13.350 total 3108
09:53:13.352 + ls -la common
09:53:13.352 drwxr-xr-x 16 go go    4096 May 24 09:53 .
09:53:13.353 + cat common/settings.py
09:53:13.353 drwxr-xr-x  3 go go    4096 May 24 09:33 ..
09:53:13.353 drwxr-xr-x  2 go go    4096 May 24 09:53 common
09:53:13.353 -rwxr-xr-x  1 go go    1597 May 24 09:33 create_update_image.sh
09:53:13.353 drwxr-xr-x  4 go go    4096 May 24 09:33 deb_update
09:53:13.354 -rw-r--r--  1 go go  913408 May 24 09:33 deb_update.upd
09:53:13.354 drwxr-xr-x  2 go go    4096 May 24 09:33 franca_idl
09:53:13.354 drwxr-xr-x  8 go go    4096 May 24 09:53 .git
09:53:13.354 -rw-r--r--  1 go go     846 May 24 09:33 .gitignore
09:53:13.354 drwxr-xr-x  2 go go    4096 May 24 09:33 hmi
09:53:13.355 -rw-r--r--  1 go go   15977 May 24 09:33 LICENSE
09:53:13.355 drwxr-xr-x  2 go go    4096 May 24 09:33 lifecycle_manager
09:53:13.355 drwxr-xr-x  2 go go    4096 May 24 09:33 module_loader_ecu1
09:53:13.355 drwxr-xr-x  2 go go    4096 May 24 09:33 package_manager
09:53:13.355 drwxr-xr-x  2 go go    4096 May 24 09:33 partition_manager
09:53:13.356 -rw-r--r--  1 go go   16987 May 24 09:33 README.md
09:53:13.356 drwxr-xr-x  4 go go    4096 May 24 09:33 rpm_update
09:53:13.356 -rw-r--r--  1 go go 1003520 May 24 09:33 rpm_update.upd
09:53:13.356 drwxr-xr-x  4 go go    4096 May 24 09:33 sample_update
09:53:13.356 -rw-r--r--  1 go go    4096 May 24 09:33 sample_update.upd
09:53:13.360 drwxr-xr-x  2 go go    4096 May 24 09:33 software_loading_manager
09:53:13.361 -rw-r--r--  1 go go  585058 May 24 09:33 Software Management Genivi.pdf
09:53:13.361 drwxr-xr-x  2 go go    4096 May 24 09:33 sota_client
09:53:13.361 -rwxr-xr-x  1 go go    3808 May 24 09:33 start_swm.sh
09:53:13.361 -rw-r--r--  1 go go   88921 May 24 09:33 swm_illustration.png
09:53:13.362 -rw-r--r--  1 go go  468741 May 24 09:33 swm_illustration.pptx
09:53:13.362 drwxr-xr-x  2 go go    4096 May 24 09:33 use_case_diagrams
09:53:13.362 total 44
09:53:13.362 drwxr-xr-x  2 go go  4096 May 24 09:53 .
09:53:13.362 drwxr-xr-x 16 go go  4096 May 24 09:53 ..
09:53:13.362 -rw-r--r--  1 go go 18453 May 24 09:33 database.py
09:53:13.363 -rwxr-xr-x  1 go go 11183 May 24 09:53 settings.py
09:53:13.363 -rw-r--r--  1 go go  3421 May 24 09:33 swm.py
09:53:13.363 # -*- coding: utf-8 -*-
09:53:13.363 """ Database library to store update progress and results.
09:53:13.363 
09:53:13.364 This module provides the configuration settings for Software Management.
09:53:13.364 
09:53:13.364 (c) 2015, 2016 - Jaguar Land Rover.
09:53:13.364 Mozilla Public License 2.0
09:53:13.364 """
09:53:13.365 
09:53:13.365 
09:53:13.365 # Build paths inside the project like this: os.path.join(BASE_DIR, ...)
09:53:13.365 # Don't change this.
09:53:13.365 import os
09:53:13.366 BASE_DIR = os.path.dirname(os.path.dirname(__file__))
09:53:13.366 
09:53:13.366 
09:53:13.366 # Simulation
09:53:13.366 # If simulation is enabled all modules only simulate their operations
09:53:13.367 # rather than carrying them out. Simulation simply means outputting
09:53:13.367 # progress to the logging facilities.
09:53:13.367 SWM_SIMULATION = True
09:53:13.367 SWM_SIMULATION_WAIT = 5
09:53:13.367 
09:53:13.367 # Database Settings
09:53:13.368 # SWM operations and their results are stored in a SQLite database.
09:53:13.368 DB_URL = "sqlite:/tmp/swlm.sqlite"
09:53:13.368 
09:53:13.368 # Logging settings
09:53:13.369 LOGGER = 'swm.default'
09:53:13.369 LOGFILE = os.path.join(BASE_DIR, 'swm.log')
09:53:13.369 
09:53:13.369 
09:53:13.369 # Don't change anything below this line unless you are familiar with the
09:53:13.369 # settings.
09:53:13.370 
09:53:13.370 # Logger configuration
09:53:13.370 import logging.config
09:53:13.370 LOGGING_CONFIG = {
09:53:13.371     'version': 1,
09:53:13.371     'disable_existing_loggers': False,
09:53:13.371     'formatters': {
09:53:13.371         'verbose': {
09:53:13.372             'format': '%(levelname)s %(asctime)s %(module)s %(process)d %(thread)d %(message)s'
09:53:13.372         },
09:53:13.372         'simple': {
09:53:13.372             'format': '%(levelname)s %(message)s'
09:53:13.372         },
09:53:13.373     },
09:53:13.373     'handlers': {
09:53:13.373         'null': {
09:53:13.373             'level': 'DEBUG',
09:53:13.373             'class': 'logging.NullHandler',
09:53:13.374         },
09:53:13.374         'console': {
09:53:13.374              'level': 'DEBUG',
09:53:13.374              'class': 'logging.StreamHandler',
09:53:13.374              'formatter': 'simple',
09:53:13.375         },
09:53:13.375         'file': {
09:53:13.375             'level': 'DEBUG',
09:53:13.375             'class': 'logging.FileHandler',
09:53:13.375             'filename': LOGFILE,
09:53:13.375             'formatter': 'verbose',
09:53:13.376         },
09:53:13.376     },
09:53:13.376     'loggers': {
09:53:13.376         'swm.default': {
09:53:13.376             'handlers': ['file','console'],
09:53:13.377             'level': 'DEBUG',
09:53:13.377             'propagate': False,
09:53:13.377         },
09:53:13.377         'swm.console': {
09:53:13.377             'handlers': ['console'],
09:53:13.378             'level': 'DEBUG',
09:53:13.378             'propagate': False,
09:53:13.378         },
09:53:13.378         'swm.file': {
09:53:13.378             'handlers': ['file'],
09:53:13.379             'level': 'DEBUG',
09:53:13.379             'propagate': False,
09:53:13.379         },
09:53:13.379     },
09:53:13.379 }
09:53:13.379 logging.config.dictConfig(LOGGING_CONFIG)
09:53:13.380 
09:53:13.380 
09:53:13.380 # Definitions of the operations that can be specified in an update manifest.
09:53:13.380 # This is a dictionary with the name of the operation as the key. The format is:
09:53:13.381 #
09:53:13.381 # "installPackage": (                       # name of the operation as used in the manifest
09:53:13.381 #     "org.genivi.PackageManager",          # the dbus object to call for the operation
09:53:13.381 #     "installPackage",                     # the dbus method for the operation provided by the dbus object
09:53:13.381 #     [                                     # array with argument tuples extracted from the manifest
09:53:13.381 #         ("image", None),                  # the first value is the argument name, the second the default value
09:53:13.382 #         ("blacklistedPackages", [])       # if the default value is None, the argument is mandatory
09:53:13.382 #     ],
09:53:13.382 #     [                                     # array with parameter tuples
09:53:13.382 #         ("timeEstimate", 5000),           # default time estimate for the operation
09:53:13.383 #         ("onFailure", "abort")            # default action if operation fails: abort or continue
09:53:13.383 #    ]
09:53:13.383 # )
09:53:13.383 #
09:53:13.384 # Default values can explictly be overridden by the manifest.
09:53:13.384 #
09:53:13.384 # Note: The number and type of paramters must match the signature of the dbus method.
09:53:13.384 #
09:53:13.385 
09:53:13.385 OPERATIONS = {
09:53:13.385     "installPackage": (
09:53:13.385         "org.genivi.PackageManager",
09:53:13.386         "installPackage",
09:53:13.386         [
09:53:13.386             ("image", None),
09:53:13.386             ("blacklistedPackages", [])     # default list of package names that are blackedlisted by default
09:53:13.386         ],
09:53:13.387         [
09:53:13.387             ("timeEstimate", 5000),         # default time estimate for the operation
09:53:13.387             ("onFailure", "abort")          # default action if operation fails: abort or continue
09:53:13.387         ]
09:53:13.388     ),
09:53:13.388 
09:53:13.388     "upgradePackage": (
09:53:13.388         "org.genivi.PackageManager",
09:53:13.388         "upgradePackage",
09:53:13.389         [
09:53:13.389             ("image", None),
09:53:13.389             ("blacklistedPackages", []),    # enter list of package names that are blackedlisted by default
09:53:13.389             ("allowDowngrade", False)       # downgrades are disabled by default
09:53:13.390         ],
09:53:13.390         [
09:53:13.390             ("timeEstimate", 5000),         # default time estimate for the operation
09:53:13.390             ("onFailure", "abort")          # default action if operation fails: abort or continue
09:53:13.391         ]
09:53:13.391     ),
09:53:13.391 
09:53:13.391     "removePackage": (
09:53:13.391         "org.genivi.PackageManager",
09:53:13.392         "removePackage",
09:53:13.392         [
09:53:13.392             ("packageId", None)
09:53:13.392         ],
09:53:13.393         [
09:53:13.393             ("timeEstimate", 5000),         # default time estimate for the operation
09:53:13.393             ("onFailure", "abort")          # default action if operation fails: abort or continue
09:53:13.393         ]
09:53:13.393     ),
09:53:13.394 
09:53:13.394     "startComponents": (
09:53:13.394         "org.genivi.LifecycleManager",
09:53:13.394         "startComponents",
09:53:13.394         [
09:53:13.395             ("components", None) 
09:53:13.395         ],
09:53:13.395         [
09:53:13.395             ("timeEstimate", 3000),         # default time estimate for the operation
09:53:13.395             ("onFailure", "abort")          # default action if operation fails: abort or continue
09:53:13.395         ]
09:53:13.396     ),
09:53:13.396 
09:53:13.396     "stopComponents": (
09:53:13.396         "org.genivi.LifecycleManager",
09:53:13.396         "stopComponents",
09:53:13.397         [
09:53:13.397             ("components", None)
09:53:13.397         ],
09:53:13.397         [
09:53:13.397             ("timeEstimate", 3000),         # default time estimate for the operation
09:53:13.398             ("onFailure", "abort")          # default action if operation fails: abort or continue
09:53:13.398         ]
09:53:13.398     ),
09:53:13.398 
09:53:13.398     "reboot": (
09:53:13.399         "org.genivi.LifecycleManager",
09:53:13.399         "reboot",
09:53:13.399         [
09:53:13.399             ("bootParameters", "")
09:53:13.399         ],
09:53:13.399         [
09:53:13.400             ("timeEstimate", 3000),         # default time estimate for the operation
09:53:13.400             ("onFailure", "continue")       # default action if operation fails: abort or continue
09:53:13.400         ]
09:53:13.400     ),
09:53:13.400 
09:53:13.401     "createDiskPartition": (
09:53:13.401         "org.genivi.PartitionManager",
09:53:13.401         "createDiskPartition",
09:53:13.401         [
09:53:13.401             ("disk", None),
09:53:13.402             ("partitionNumber", None),
09:53:13.402             ("type", None),
09:53:13.402             ("start", None),
09:53:13.402             ("size", None),
09:53:13.402             ("guid", ""),
09:53:13.402             ("name", "")
09:53:13.403         ],
09:53:13.403         [
09:53:13.403             ("timeEstimate", 10000),        # default time estimate for the operation
09:53:13.403             ("onFailure", "abort")          # default action if operation fails: abort or continue
09:53:13.403         ]
09:53:13.404     ),
09:53:13.404             
09:53:13.404     "resizeDiskPartition": (
09:53:13.404         "org.genivi.PartitionManager",
09:53:13.404         "resizeDiskPartition",
09:53:13.405         [
09:53:13.405             ("disk", None),
09:53:13.405             ("partitionNumber", None),
09:53:13.405             ("start", None),
09:53:13.405             ("size", None),
09:53:13.406             ("blacklistedPartitions", [])
09:53:13.406         ],
09:53:13.406         [
09:53:13.406             ("timeEstimate", 10000),        # default time estimate for the operation
09:53:13.406             ("onFailure", "abort")          # default action if operation fails: abort or continue
09:53:13.407         ]
09:53:13.407     ),
09:53:13.407 
09:53:13.407     "deleteDiskPartition": (
09:53:13.407         "org.genivi.PartitionManager",
09:53:13.408         "deleteDiskPartition",
09:53:13.408         [
09:53:13.408             ("disk", None),
09:53:13.408             ("partitionNnumber", None),
09:53:13.409             ("blacklistedPartitions", [])
09:53:13.409         ],
09:53:13.409         [
09:53:13.409             ("timeEstimate", 10000),        # default time estimate for the operation
09:53:13.409             ("onFailure", "abort")          # default action if operation fails: abort or continue
09:53:13.409         ]
09:53:13.410     ),
09:53:13.410 
09:53:13.410     "writeDiskPartition": (
09:53:13.410         "org.genivi.PartitionManager",
09:53:13.410         "writeDiskPartition",
09:53:13.411         [
09:53:13.411             ("disk", None),
09:53:13.411             ("partitionNumber", None),
09:53:13.411             ("image", None),
09:53:13.411             ("blacklistedPartitions", [])
09:53:13.412         ],
09:53:13.412         [
09:53:13.412             ("timeEstimate", 10000),        # default time estimate for the operation
09:53:13.412             ("onFailure", "abort")          # default action if operation fails: abort or continue
09:53:13.412         ]
09:53:13.413     ),
09:53:13.413         
09:53:13.413     "patchDiskPartition": (
09:53:13.413         "org.genivi.PartitionManager",
09:53:13.413         "patchDiskPartition",
09:53:13.413         [
09:53:13.414             ("disk", None),
09:53:13.414             ("partition_number", None),
09:53:13.414             ("image", None),
09:53:13.414             ("blacklistedPartitions", [])
09:53:13.414         ],
09:53:13.415         [
09:53:13.415             ("timeEstimate", 10000),        # default time estimate for the operation
09:53:13.415             ("onFailure", "abort")          # default action if operation fails: abort or continue
09:53:13.415         ]
09:53:13.415     ),
09:53:13.416         
09:53:13.416     # FIXME: We need to find a specific module loader
09:53:13.416     #        that handles the target module. 
09:53:13.416     #        org.genivi.module_loader needs to be replaced
09:53:13.416     #        by org.genivi.module_loader_ecu1
09:53:13.417     #        This should be done programmatically
09:53:13.417     "flashModuleFirmwareEcu1": (
09:53:13.417         "org.genivi.ModuleLoaderEcu1",
09:53:13.417         "flashModuleFirmware",
09:53:13.417         [
09:53:13.418             ("image", None),
09:53:13.418             ("blacklistedFirmware", []),
09:53:13.418             ("allowDowngrade", False)
09:53:13.418         ],
09:53:13.418         [
09:53:13.418             ("timeEstimate", 5000),         # default time estimate for the operation
09:53:13.419             ("onFailure", "abort")          # default action if operation fails: abort or continue
09:53:13.419         ]
09:53:13.419     )
09:53:13.419 }
09:53:13.419 
09:53:13.420 
09:53:13.420 # Filesystem Commands
09:53:13.420 #
09:53:13.420 # SWM uses squashfs for update files that are mounted. Typically, only
09:53:13.420 # root is allowed to mount file systems. However, FUSE allows user
09:53:13.421 # mounting. The user-mount squashfs there is the squashfuse. It is not
09:53:13.421 # standard with any Linux distro but can be downloaded and installed from
09:53:13.421 # https://github.com/vasi/squashfuse. Squashfuse requires FUSE to be installed
09:53:13.421 # on the system.
09:53:13.421 #
09:53:13.422 # If a command does not need any arguments, other than the archive and the mount
09:53:13.422 # point which are provided programmatically, set the variable to None. Do not use
09:53:13.422 # an empty string or a string with spaces.
09:53:13.422 #
09:53:13.422 SQUASHFS_MOUNT_POINT = "/tmp/swlm"
09:53:13.423 SQUASHFS_FUSE = SWM_SIMULATION
09:53:13.423 if SQUASHFS_FUSE:
09:53:13.423     # FUSE mount
09:53:13.423     SQUASHFS_MOUNT_CMD = ["/usr/local/bin/squashfuse"]
09:53:13.423     SQUASHFS_UNMOUNT_CMD = ["/bin/fusermount", "-u"]
09:53:13.423 else:
09:53:13.424     # Regular mount as root 
09:53:13.424     SQUASHFS_MOUNT_CMD = ["/bin/mount"]
09:53:13.424     SQUASHFS_UNMOUNT_CMD = ["/bin/umount"]
09:53:13.424 
09:53:13.424 
09:53:13.425 # Package Management Commands
09:53:13.425 #
09:53:13.425 # SWM uses the platform's package management systems to install, upgrade and
09:53:13.425 # remove software packages.
09:53:13.425 #
09:53:13.426 PACKAGE_MANAGER = "rpm"
09:53:13.426 if PACKAGE_MANAGER == "rpm":
09:53:13.426     PKGMGR_INSTALL_CMD = ["rpm", "--install"]
09:53:13.426     PKGMGR_UPGRADE_CMD = ["rpm", "--upgrade", "--oldpackage"]
09:53:13.426     PKGMGR_REMOVE_CMD = ["rpm", "--erase"]
09:53:13.427     PKGMGR_LIST_CMD = ["rpm", "--query", "--all"]
09:53:13.427     PKGMGR_CHECK_CMD = ["rpm", "--query"]
09:53:13.427     PKGMGR_DEL_ARCH = '.'
09:53:13.427     PKGMGR_DEL_REL = '-'
09:53:13.427     PKGMGR_DEL_VER = '-'
09:53:13.427 elif PACKAGE_MANAGER == "deb":
09:53:13.428     PKGMGR_INSTALL_CMD = ["dpkg", "--install"]
09:53:13.428     PKGMGR_UPGRADE_CMD = ["dpkg", "--install"]
09:53:13.428     PKGMGR_REMOVE_CMD = ["dpkg", "--purge"]
09:53:13.428     PKGMGR_LIST_CMD = ["dpkg-query", "--show", "--showformat", "'${Package}-${Version}.${Architecture}\n'"]
09:53:13.428     PKGMGR_CHECK_CMD = ["dpkg-query", "--list"]
09:53:13.429     PKGMGR_DEL_ARCH = '_'
09:53:13.429     PKGMGR_DEL_REL = '-'
09:53:13.429     PKGMGR_DEL_VER = '_'
09:53:13.429 else:
09:53:13.429     PKGMGR_INSTALL_CMD = ["echo", "Incorrect package manager defined."]
09:53:13.430     PKGMGR_UPGRADE_CMD = ["echo", "Incorrect package manager defined."]
09:53:13.430     PKGMGR_REMOVE_CMD = ["echo", "Incorrect package manager defined."]
09:53:13.430     PKGMGR_LIST_CMD = ["echo", "Incorrect package manager defined."]
09:53:13.430     PKGMGR_CHECK_CMD = ["echo", "Incorrect package manager defined."]
09:53:13.430   
09:53:13.454 [go] Current job status: passed.

09:53:13.454 [go] Start to execute task: <exec command="which" >
<arg>python</arg>
</exec>.
09:53:13.490 [go] Current job status: failed.

09:53:13.500 [go] Start to create properties software-loading-manager/5/runSWLMPocStage/1/runSWLMPocJob on ca921f3ae51a [/var/lib/go-agent]
09:53:13.500 [go] Start to upload software-loading-manager/5/runSWLMPocStage/1/runSWLMPocJob on ca921f3ae51a [/var/lib/go-agent]
09:53:13.603 [go] Job completed software-loading-manager/5/runSWLMPocStage/1/runSWLMPocJob on ca921f3ae51a [/var/lib/go-agent]
```

<!-- EOF -->
