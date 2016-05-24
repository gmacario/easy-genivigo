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

**TODO**: Make sure the pipeline is run on an agent with Python installed.

### Run pipeline `software-loading-manager`

Browse `${GOCD_URL}`, then click **PIPELINES**.

* Unpause pipeline `software-loading-manager`, then click the "Trigger" button.

Wait until pipeline `software-loading-manager` finishes, then review build log.

**FIXME**: The pipeline fails in `runSWLMPocStage` - see https://at.projects.genivi.org/jira/browse/TOOL-23.

```
12:05:57.083 [go] Job Started: 2016-05-24 12:05:57 UTC

12:05:57.083 [go] Start to prepare software-loading-manager/6/runSWLMPocStage/1/runSWLMPocJob on adb59c386d21 [/var/lib/go-agent]
12:05:57.097 [go] Start to update materials.

12:05:57.097 [go] Start updating files at revision 0bda6ecfbd7ae7b7a03497895d3c2db51010c404 from https://github.com/GENIVI/genivi_swm
12:05:57.203 [GIT] Fetch and reset in working directory pipelines/software-loading-manager
12:05:57.203 [GIT] Cleaning all unversioned files in working copy
12:05:57.209 [GIT] Cleaning submodule configurations in .git/config
12:05:57.211 [GIT] Fetching changes
12:05:58.035 [GIT] Performing git gc
12:05:58.138 [GIT] Updating working copy to revision 0bda6ecfbd7ae7b7a03497895d3c2db51010c404
12:05:58.143 HEAD is now at 0bda6ec Package Manager Real Operations
12:05:58.243 [GIT] Removing modified files in submodules
12:05:58.267 [GIT] Cleaning all unversioned files in working copy
12:05:58.472 [go] Done.

12:05:58.473 [go] setting environment variable 'GO_SERVER_URL' to value 'https://goserver:8154/go/'
12:05:58.473 [go] setting environment variable 'GO_TRIGGER_USER' to value 'changes'
12:05:58.473 [go] setting environment variable 'GO_PIPELINE_NAME' to value 'software-loading-manager'
12:05:58.473 [go] setting environment variable 'GO_PIPELINE_COUNTER' to value '6'
12:05:58.473 [go] setting environment variable 'GO_PIPELINE_LABEL' to value '6'
12:05:58.473 [go] setting environment variable 'GO_STAGE_NAME' to value 'runSWLMPocStage'
12:05:58.473 [go] setting environment variable 'GO_STAGE_COUNTER' to value '1'
12:05:58.473 [go] setting environment variable 'GO_JOB_NAME' to value 'runSWLMPocJob'
12:05:58.473 [go] setting environment variable 'GO_REVISION' to value '0bda6ecfbd7ae7b7a03497895d3c2db51010c404'
12:05:58.474 [go] setting environment variable 'GO_TO_REVISION' to value '0bda6ecfbd7ae7b7a03497895d3c2db51010c404'
12:05:58.474 [go] setting environment variable 'GO_FROM_REVISION' to value '0bda6ecfbd7ae7b7a03497895d3c2db51010c404'

12:05:58.498 [go] Start to build software-loading-manager/6/runSWLMPocStage/1/runSWLMPocJob on adb59c386d21 [/var/lib/go-agent]
12:05:58.498 [go] Current job status: passed.

12:05:58.498 [go] Start to execute task: <exec command="sed" >
<arg>-i</arg>
<arg>-e</arg>
<arg>s/^SWM_SIMULATION\ \=.*$/SWM_SIMULATION = True/</arg>
<arg>common/settings.py</arg>
</exec>.
12:05:58.504 [go] Current job status: passed.

12:05:58.504 [go] Start to execute task: <exec command="bash" >
<arg>-xe</arg>
<arg>-c</arg>
<arg>ls -la && ls -la common && cat common/settings.py</arg>
</exec>.
12:05:58.508 + ls -la
12:05:58.517 total 3108
12:05:58.517 + ls -la common
12:05:58.517 drwxr-xr-x 16 go go    4096 May 24 12:05 .
12:05:58.518 drwxr-xr-x  3 go go    4096 May 24 12:05 ..
12:05:58.518 drwxr-xr-x  8 go go    4096 May 24 12:05 .git
12:05:58.519 -rw-r--r--  1 go go     846 May 24 12:05 .gitignore
12:05:58.519 -rw-r--r--  1 go go   15977 May 24 12:05 LICENSE
12:05:58.520 + cat common/settings.py
12:05:58.520 -rw-r--r--  1 go go   16987 May 24 12:05 README.md
12:05:58.520 -rw-r--r--  1 go go  585058 May 24 12:05 Software Management Genivi.pdf
12:05:58.521 drwxr-xr-x  2 go go    4096 May 24 12:05 common
12:05:58.521 -rwxr-xr-x  1 go go    1597 May 24 12:05 create_update_image.sh
12:05:58.522 drwxr-xr-x  4 go go    4096 May 24 12:05 deb_update
12:05:58.522 -rw-r--r--  1 go go  913408 May 24 12:05 deb_update.upd
12:05:58.522 drwxr-xr-x  2 go go    4096 May 24 12:05 franca_idl
12:05:58.523 drwxr-xr-x  2 go go    4096 May 24 12:05 hmi
12:05:58.523 drwxr-xr-x  2 go go    4096 May 24 12:05 lifecycle_manager
12:05:58.524 drwxr-xr-x  2 go go    4096 May 24 12:05 module_loader_ecu1
12:05:58.524 drwxr-xr-x  2 go go    4096 May 24 12:05 package_manager
12:05:58.525 drwxr-xr-x  2 go go    4096 May 24 12:05 partition_manager
12:05:58.525 drwxr-xr-x  4 go go    4096 May 24 12:05 rpm_update
12:05:58.525 -rw-r--r--  1 go go 1003520 May 24 12:05 rpm_update.upd
12:05:58.526 drwxr-xr-x  4 go go    4096 May 24 12:05 sample_update
12:05:58.526 -rw-r--r--  1 go go    4096 May 24 12:05 sample_update.upd
12:05:58.527 drwxr-xr-x  2 go go    4096 May 24 12:05 software_loading_manager
12:05:58.527 drwxr-xr-x  2 go go    4096 May 24 12:05 sota_client
12:05:58.528 -rwxr-xr-x  1 go go    3808 May 24 12:05 start_swm.sh
12:05:58.528 -rw-r--r--  1 go go   88921 May 24 12:05 swm_illustration.png
12:05:58.528 -rw-r--r--  1 go go  468741 May 24 12:05 swm_illustration.pptx
12:05:58.529 drwxr-xr-x  2 go go    4096 May 24 12:05 use_case_diagrams
12:05:58.529 total 44
12:05:58.530 drwxr-xr-x  2 go go  4096 May 24 12:05 .
12:05:58.530 drwxr-xr-x 16 go go  4096 May 24 12:05 ..
12:05:58.531 -rw-r--r--  1 go go 18453 May 24 12:05 database.py
12:05:58.531 -rwxr-xr-x  1 go go 11183 May 24 12:05 settings.py
12:05:58.531 -rw-r--r--  1 go go  3421 May 24 12:05 swm.py
12:05:58.532 # -*- coding: utf-8 -*-
12:05:58.532 """ Database library to store update progress and results.
12:05:58.533 
12:05:58.533 This module provides the configuration settings for Software Management.
12:05:58.533 
12:05:58.534 (c) 2015, 2016 - Jaguar Land Rover.
12:05:58.534 Mozilla Public License 2.0
12:05:58.534 """
12:05:58.535 
12:05:58.535 
12:05:58.536 # Build paths inside the project like this: os.path.join(BASE_DIR, ...)
12:05:58.536 # Don't change this.
12:05:58.536 import os
12:05:58.537 BASE_DIR = os.path.dirname(os.path.dirname(__file__))
12:05:58.537 
12:05:58.538 
12:05:58.538 # Simulation
12:05:58.538 # If simulation is enabled all modules only simulate their operations
12:05:58.539 # rather than carrying them out. Simulation simply means outputting
12:05:58.539 # progress to the logging facilities.
12:05:58.540 SWM_SIMULATION = True
12:05:58.540 SWM_SIMULATION_WAIT = 5
12:05:58.541 
12:05:58.541 # Database Settings
12:05:58.541 # SWM operations and their results are stored in a SQLite database.
12:05:58.542 DB_URL = "sqlite:/tmp/swlm.sqlite"
12:05:58.543 
12:05:58.543 # Logging settings
12:05:58.544 LOGGER = 'swm.default'
12:05:58.544 LOGFILE = os.path.join(BASE_DIR, 'swm.log')
12:05:58.545 
12:05:58.545 
12:05:58.545 # Don't change anything below this line unless you are familiar with the
12:05:58.546 # settings.
12:05:58.546 
12:05:58.547 # Logger configuration
12:05:58.547 import logging.config
12:05:58.547 LOGGING_CONFIG = {
12:05:58.548     'version': 1,
12:05:58.548     'disable_existing_loggers': False,
12:05:58.549     'formatters': {
12:05:58.549         'verbose': {
12:05:58.550             'format': '%(levelname)s %(asctime)s %(module)s %(process)d %(thread)d %(message)s'
12:05:58.550         },
12:05:58.551         'simple': {
12:05:58.551             'format': '%(levelname)s %(message)s'
12:05:58.551         },
12:05:58.552     },
12:05:58.552     'handlers': {
12:05:58.553         'null': {
12:05:58.553             'level': 'DEBUG',
12:05:58.554             'class': 'logging.NullHandler',
12:05:58.554         },
12:05:58.554         'console': {
12:05:58.555              'level': 'DEBUG',
12:05:58.555              'class': 'logging.StreamHandler',
12:05:58.555              'formatter': 'simple',
12:05:58.556         },
12:05:58.556         'file': {
12:05:58.557             'level': 'DEBUG',
12:05:58.557             'class': 'logging.FileHandler',
12:05:58.557             'filename': LOGFILE,
12:05:58.558             'formatter': 'verbose',
12:05:58.558         },
12:05:58.559     },
12:05:58.559     'loggers': {
12:05:58.559         'swm.default': {
12:05:58.560             'handlers': ['file','console'],
12:05:58.560             'level': 'DEBUG',
12:05:58.561             'propagate': False,
12:05:58.561         },
12:05:58.561         'swm.console': {
12:05:58.562             'handlers': ['console'],
12:05:58.562             'level': 'DEBUG',
12:05:58.562             'propagate': False,
12:05:58.563         },
12:05:58.563         'swm.file': {
12:05:58.564             'handlers': ['file'],
12:05:58.564             'level': 'DEBUG',
12:05:58.564             'propagate': False,
12:05:58.565         },
12:05:58.565     },
12:05:58.566 }
12:05:58.566 logging.config.dictConfig(LOGGING_CONFIG)
12:05:58.567 
12:05:58.567 
12:05:58.567 # Definitions of the operations that can be specified in an update manifest.
12:05:58.568 # This is a dictionary with the name of the operation as the key. The format is:
12:05:58.568 #
12:05:58.569 # "installPackage": (                       # name of the operation as used in the manifest
12:05:58.569 #     "org.genivi.PackageManager",          # the dbus object to call for the operation
12:05:58.570 #     "installPackage",                     # the dbus method for the operation provided by the dbus object
12:05:58.570 #     [                                     # array with argument tuples extracted from the manifest
12:05:58.570 #         ("image", None),                  # the first value is the argument name, the second the default value
12:05:58.571 #         ("blacklistedPackages", [])       # if the default value is None, the argument is mandatory
12:05:58.571 #     ],
12:05:58.572 #     [                                     # array with parameter tuples
12:05:58.572 #         ("timeEstimate", 5000),           # default time estimate for the operation
12:05:58.573 #         ("onFailure", "abort")            # default action if operation fails: abort or continue
12:05:58.573 #    ]
12:05:58.573 # )
12:05:58.574 #
12:05:58.574 # Default values can explictly be overridden by the manifest.
12:05:58.575 #
12:05:58.575 # Note: The number and type of paramters must match the signature of the dbus method.
12:05:58.576 #
12:05:58.576 
12:05:58.576 OPERATIONS = {
12:05:58.577     "installPackage": (
12:05:58.577         "org.genivi.PackageManager",
12:05:58.578         "installPackage",
12:05:58.578         [
12:05:58.579             ("image", None),
12:05:58.579             ("blacklistedPackages", [])     # default list of package names that are blackedlisted by default
12:05:58.579         ],
12:05:58.580         [
12:05:58.580             ("timeEstimate", 5000),         # default time estimate for the operation
12:05:58.581             ("onFailure", "abort")          # default action if operation fails: abort or continue
12:05:58.581         ]
12:05:58.581     ),
12:05:58.582 
12:05:58.582     "upgradePackage": (
12:05:58.582         "org.genivi.PackageManager",
12:05:58.583         "upgradePackage",
12:05:58.583         [
12:05:58.584             ("image", None),
12:05:58.584             ("blacklistedPackages", []),    # enter list of package names that are blackedlisted by default
12:05:58.584             ("allowDowngrade", False)       # downgrades are disabled by default
12:05:58.585         ],
12:05:58.585         [
12:05:58.585             ("timeEstimate", 5000),         # default time estimate for the operation
12:05:58.586             ("onFailure", "abort")          # default action if operation fails: abort or continue
12:05:58.586         ]
12:05:58.587     ),
12:05:58.587 
12:05:58.587     "removePackage": (
12:05:58.588         "org.genivi.PackageManager",
12:05:58.588         "removePackage",
12:05:58.588         [
12:05:58.589             ("packageId", None)
12:05:58.589         ],
12:05:58.590         [
12:05:58.590             ("timeEstimate", 5000),         # default time estimate for the operation
12:05:58.590             ("onFailure", "abort")          # default action if operation fails: abort or continue
12:05:58.591         ]
12:05:58.591     ),
12:05:58.591 
12:05:58.592     "startComponents": (
12:05:58.592         "org.genivi.LifecycleManager",
12:05:58.592         "startComponents",
12:05:58.593         [
12:05:58.593             ("components", None) 
12:05:58.594         ],
12:05:58.594         [
12:05:58.594             ("timeEstimate", 3000),         # default time estimate for the operation
12:05:58.595             ("onFailure", "abort")          # default action if operation fails: abort or continue
12:05:58.595         ]
12:05:58.595     ),
12:05:58.596 
12:05:58.596     "stopComponents": (
12:05:58.596         "org.genivi.LifecycleManager",
12:05:58.597         "stopComponents",
12:05:58.597         [
12:05:58.597             ("components", None)
12:05:58.598         ],
12:05:58.598         [
12:05:58.598             ("timeEstimate", 3000),         # default time estimate for the operation
12:05:58.599             ("onFailure", "abort")          # default action if operation fails: abort or continue
12:05:58.599         ]
12:05:58.599     ),
12:05:58.600 
12:05:58.600     "reboot": (
12:05:58.600         "org.genivi.LifecycleManager",
12:05:58.600         "reboot",
12:05:58.601         [
12:05:58.601             ("bootParameters", "")
12:05:58.601         ],
12:05:58.602         [
12:05:58.602             ("timeEstimate", 3000),         # default time estimate for the operation
12:05:58.602             ("onFailure", "continue")       # default action if operation fails: abort or continue
12:05:58.603         ]
12:05:58.603     ),
12:05:58.603 
12:05:58.603     "createDiskPartition": (
12:05:58.604         "org.genivi.PartitionManager",
12:05:58.604         "createDiskPartition",
12:05:58.604         [
12:05:58.605             ("disk", None),
12:05:58.605             ("partitionNumber", None),
12:05:58.605             ("type", None),
12:05:58.606             ("start", None),
12:05:58.606             ("size", None),
12:05:58.606             ("guid", ""),
12:05:58.606             ("name", "")
12:05:58.607         ],
12:05:58.607         [
12:05:58.607             ("timeEstimate", 10000),        # default time estimate for the operation
12:05:58.608             ("onFailure", "abort")          # default action if operation fails: abort or continue
12:05:58.608         ]
12:05:58.608     ),
12:05:58.609             
12:05:58.609     "resizeDiskPartition": (
12:05:58.609         "org.genivi.PartitionManager",
12:05:58.610         "resizeDiskPartition",
12:05:58.610         [
12:05:58.610             ("disk", None),
12:05:58.611             ("partitionNumber", None),
12:05:58.611             ("start", None),
12:05:58.611             ("size", None),
12:05:58.612             ("blacklistedPartitions", [])
12:05:58.612         ],
12:05:58.612         [
12:05:58.612             ("timeEstimate", 10000),        # default time estimate for the operation
12:05:58.613             ("onFailure", "abort")          # default action if operation fails: abort or continue
12:05:58.613         ]
12:05:58.613     ),
12:05:58.614 
12:05:58.614     "deleteDiskPartition": (
12:05:58.614         "org.genivi.PartitionManager",
12:05:58.615         "deleteDiskPartition",
12:05:58.615         [
12:05:58.615             ("disk", None),
12:05:58.616             ("partitionNnumber", None),
12:05:58.616             ("blacklistedPartitions", [])
12:05:58.616         ],
12:05:58.617         [
12:05:58.617             ("timeEstimate", 10000),        # default time estimate for the operation
12:05:58.617             ("onFailure", "abort")          # default action if operation fails: abort or continue
12:05:58.617         ]
12:05:58.618     ),
12:05:58.618 
12:05:58.618     "writeDiskPartition": (
12:05:58.619         "org.genivi.PartitionManager",
12:05:58.619         "writeDiskPartition",
12:05:58.619         [
12:05:58.620             ("disk", None),
12:05:58.620             ("partitionNumber", None),
12:05:58.621             ("image", None),
12:05:58.621             ("blacklistedPartitions", [])
12:05:58.621         ],
12:05:58.622         [
12:05:58.622             ("timeEstimate", 10000),        # default time estimate for the operation
12:05:58.622             ("onFailure", "abort")          # default action if operation fails: abort or continue
12:05:58.622         ]
12:05:58.623     ),
12:05:58.623         
12:05:58.623     "patchDiskPartition": (
12:05:58.624         "org.genivi.PartitionManager",
12:05:58.624         "patchDiskPartition",
12:05:58.624         [
12:05:58.625             ("disk", None),
12:05:58.625             ("partition_number", None),
12:05:58.626             ("image", None),
12:05:58.626             ("blacklistedPartitions", [])
12:05:58.626         ],
12:05:58.627         [
12:05:58.627             ("timeEstimate", 10000),        # default time estimate for the operation
12:05:58.627             ("onFailure", "abort")          # default action if operation fails: abort or continue
12:05:58.628         ]
12:05:58.628     ),
12:05:58.628         
12:05:58.628     # FIXME: We need to find a specific module loader
12:05:58.629     #        that handles the target module. 
12:05:58.629     #        org.genivi.module_loader needs to be replaced
12:05:58.629     #        by org.genivi.module_loader_ecu1
12:05:58.630     #        This should be done programmatically
12:05:58.630     "flashModuleFirmwareEcu1": (
12:05:58.630         "org.genivi.ModuleLoaderEcu1",
12:05:58.631         "flashModuleFirmware",
12:05:58.631         [
12:05:58.631             ("image", None),
12:05:58.632             ("blacklistedFirmware", []),
12:05:58.632             ("allowDowngrade", False)
12:05:58.632         ],
12:05:58.632         [
12:05:58.633             ("timeEstimate", 5000),         # default time estimate for the operation
12:05:58.633             ("onFailure", "abort")          # default action if operation fails: abort or continue
12:05:58.633         ]
12:05:58.634     )
12:05:58.634 }
12:05:58.634 
12:05:58.635 
12:05:58.635 # Filesystem Commands
12:05:58.635 #
12:05:58.635 # SWM uses squashfs for update files that are mounted. Typically, only
12:05:58.636 # root is allowed to mount file systems. However, FUSE allows user
12:05:58.636 # mounting. The user-mount squashfs there is the squashfuse. It is not
12:05:58.637 # standard with any Linux distro but can be downloaded and installed from
12:05:58.637 # https://github.com/vasi/squashfuse. Squashfuse requires FUSE to be installed
12:05:58.637 # on the system.
12:05:58.637 #
12:05:58.638 # If a command does not need any arguments, other than the archive and the mount
12:05:58.638 # point which are provided programmatically, set the variable to None. Do not use
12:05:58.638 # an empty string or a string with spaces.
12:05:58.639 #
12:05:58.639 SQUASHFS_MOUNT_POINT = "/tmp/swlm"
12:05:58.639 SQUASHFS_FUSE = SWM_SIMULATION
12:05:58.640 if SQUASHFS_FUSE:
12:05:58.640     # FUSE mount
12:05:58.640     SQUASHFS_MOUNT_CMD = ["/usr/local/bin/squashfuse"]
12:05:58.640     SQUASHFS_UNMOUNT_CMD = ["/bin/fusermount", "-u"]
12:05:58.641 else:
12:05:58.641     # Regular mount as root 
12:05:58.641     SQUASHFS_MOUNT_CMD = ["/bin/mount"]
12:05:58.642     SQUASHFS_UNMOUNT_CMD = ["/bin/umount"]
12:05:58.642 
12:05:58.642 
12:05:58.643 # Package Management Commands
12:05:58.643 #
12:05:58.643 # SWM uses the platform's package management systems to install, upgrade and
12:05:58.644 # remove software packages.
12:05:58.644 #
12:05:58.644 PACKAGE_MANAGER = "rpm"
12:05:58.645 if PACKAGE_MANAGER == "rpm":
12:05:58.645     PKGMGR_INSTALL_CMD = ["rpm", "--install"]
12:05:58.645     PKGMGR_UPGRADE_CMD = ["rpm", "--upgrade", "--oldpackage"]
12:05:58.645     PKGMGR_REMOVE_CMD = ["rpm", "--erase"]
12:05:58.646     PKGMGR_LIST_CMD = ["rpm", "--query", "--all"]
12:05:58.646     PKGMGR_CHECK_CMD = ["rpm", "--query"]
12:05:58.646     PKGMGR_DEL_ARCH = '.'
12:05:58.647     PKGMGR_DEL_REL = '-'
12:05:58.647     PKGMGR_DEL_VER = '-'
12:05:58.647 elif PACKAGE_MANAGER == "deb":
12:05:58.647     PKGMGR_INSTALL_CMD = ["dpkg", "--install"]
12:05:58.648     PKGMGR_UPGRADE_CMD = ["dpkg", "--install"]
12:05:58.648     PKGMGR_REMOVE_CMD = ["dpkg", "--purge"]
12:05:58.648     PKGMGR_LIST_CMD = ["dpkg-query", "--show", "--showformat", "'${Package}-${Version}.${Architecture}\n'"]
12:05:58.649     PKGMGR_CHECK_CMD = ["dpkg-query", "--list"]
12:05:58.649     PKGMGR_DEL_ARCH = '_'
12:05:58.649     PKGMGR_DEL_REL = '-'
12:05:58.650     PKGMGR_DEL_VER = '_'
12:05:58.650 else:
12:05:58.650     PKGMGR_INSTALL_CMD = ["echo", "Incorrect package manager defined."]
12:05:58.651     PKGMGR_UPGRADE_CMD = ["echo", "Incorrect package manager defined."]
12:05:58.651     PKGMGR_REMOVE_CMD = ["echo", "Incorrect package manager defined."]
12:05:58.651     PKGMGR_LIST_CMD = ["echo", "Incorrect package manager defined."]
12:05:58.651     PKGMGR_CHECK_CMD = ["echo", "Incorrect package manager defined."]
12:05:58.652   
12:05:58.726 [go] Current job status: passed.

12:05:58.727 [go] Start to execute task: <exec command="which" >
<arg>python</arg>
</exec>.
12:05:58.736 /usr/bin/python
12:05:58.836 [go] Current job status: passed.

12:05:58.836 [go] Start to execute task: <exec command="bash" >
<arg>-xe</arg>
<arg>start_swm.sh</arg>
<arg>-r</arg>
</exec>.
12:05:58.840 + PID_FNAME=/tmp/swm_pidlist
12:05:58.840 ++ lsb_release -i
12:05:58.841 ++ cut -d : -f 2
12:05:58.841 ++ tr -d ' \t\n'
12:05:59.079 + DISTRO=Ubuntu
12:05:59.079 + case "$DISTRO" in
12:05:59.079 + sed -i '0,/PACKAGE_MANAGER.*/s//PACKAGE_MANAGER = "deb"/' common/settings.py
12:05:59.083 + export PYTHONPATH=/var/lib/go-agent/pipelines/software-loading-manager/common/
12:05:59.083 + PYTHONPATH=/var/lib/go-agent/pipelines/software-loading-manager/common/
12:05:59.084 + python -c 'import settings,sys; sys.exit(settings.SWM_SIMULATION)'
12:05:59.590 [go] Current job status: failed.

12:05:59.602 [go] Start to create properties software-loading-manager/6/runSWLMPocStage/1/runSWLMPocJob on adb59c386d21 [/var/lib/go-agent]
12:05:59.602 [go] Start to upload software-loading-manager/6/runSWLMPocStage/1/runSWLMPocJob on adb59c386d21 [/var/lib/go-agent]
12:05:59.717 [go] Job completed software-loading-manager/6/runSWLMPocStage/1/runSWLMPocJob on adb59c386d21 [/var/lib/go-agent]
```

<!-- EOF -->
