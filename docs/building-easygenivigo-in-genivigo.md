Building easy-genivigo in easy-genivigo
=======================================

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
$ docker-console logs
```

### Create pipeline `build_easygenivigo`

Browse `${GOCD_URL}`, then click **Admin** > **Pipelines**.

* Click **Create a new pipeline**

Step 1: Basic Settings

* Pipeline Name: `build_easygenivigo`
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
    - Command:

      ```
      docker-compose
      ```

    - Arguments:

      ```
      build
      --pull
      ```

    - Working Directory: (none)

    then click **FINISH**.

  * Add new task > More...

    - Command:

      ```
      docker
      ```

    - Arguments:

      ```
      images
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
      ps
      -a
      ```

    - Working Directory: (none)

    then click **SAVE**.

Review pipeline `build_easygenivigo`, then click **SAVE**.

### Run pipeline `build_easygenivigo`

Browse `${GOCD_URL}`, then click **PIPELINES**.

* Start pipeline `build_easygenivigo`.

Wait until pipeline `build_easygenivigo` finishes, then review log.

**NOTE**: If the pipeline includes command `docker-compose up`, this will fail as port 8153 is already allocated by service `goserver`.

```
...
15:10:42.072 [go] Start to execute task: <exec command="docker-compose" >
<arg>up</arg>
</exec>.
15:10:42.560 Creating network "buildeasygenivigo_default" with the default driver
15:10:42.894 Creating buildeasygenivigo_goserver_1
15:10:43.238 failed to create endpoint buildeasygenivigo_goserver_1 on network buildeasygenivigo_default: Bind for 0.0.0.0:8153 failed: port is already allocated
15:10:43.323 [go] Current job status: failed.

15:10:43.346 [go] Start to create properties build_easygenivigo/2/defaultStage/1/defaultJob on 0f29bb7cc96f [/var/lib/go-agent]
15:10:43.347 [go] Start to upload build_easygenivigo/2/defaultStage/1/defaultJob on 0f29bb7cc96f [/var/lib/go-agent]
15:10:43.571 [go] Job completed build_easygenivigo/2/defaultStage/1/defaultJob on 0f29bb7cc96f [/var/lib/go-agent]
```

<!-- EOF -->
