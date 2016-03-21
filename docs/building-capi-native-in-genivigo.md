Building Common API C in easy-genivigo
======================================

**WORK-IN-PROGRESS**

Step-by-step instructions
-------------------------

```
$ git clone https://github.com/gmacario/easy-genivigo
$ cd easy-genivigo
$ ./runme.sh
```

Browse `${GENIVIGO_URL}` (example: http://192.168.99.100:8153)

* Click **Admin** > **Pipelines**.
* Click **Create a new pipeline**

Step 1: Basic Settings

* Pipeline Name: `build_capi_native`
* Pipeline Group Name: `defaultGroup`

Click **NEXT**.

Step 2: Materials

* Material Type: Git
* URL: git://git.projects.genivi.org/common-api/c-poc.git
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
    - Command:

      ```
      sh
      ```

    - Arguments:

      ```
      -c
      id && printenv | sort && pwd && ls -la
      ```

    - Working Directory: (none)

    then click **SAVE**.

  * Add new task > More...
    - Command:

      ```
      docker
      ```

    - Arguments:

      ```
      pull
      gmacario/build-capi-native
      ```

    - Working Directory: (none)

  * Add new task > More...
    - Command:

      ```
      docker
      ```

    - Arguments:

      ```
      run
      -u
      build
      -w
      /home/build
      gmacario/build-capi-native
      /bin/bash
      -c
      git clone git://git.projects.genivi.org/common-api/c-poc.git && cd c-poc && autoreconf -i && ./configure && make && sudo make install
      ```

    - Working Directory: (none)

  * Add new task > More...
    - Command:

      ```
      sh
      ```

    - Arguments:

      ```
      -c
      pwd
      ls -la
      ls -la c-poc
      ```

    - Working Directory: (none)

Click **FINISH**.

Review pipeline `build_easygenivigo`, then click **SAVE**.

Browse `${GENIVIGO_URL}`, then click **PIPELINES**.

* Start pipeline `build_capi_native`.

Wait until pipeline `build_capi_native` finishes, then review log.

```
TODO
```

<!-- EOF -->
