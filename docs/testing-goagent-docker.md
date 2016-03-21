Testing goagent-docker
======================

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
  - Command: `/bin/bash`
  - Arguments: `-c "docker info"`
  - Working Directory: (none)

Click **FINISH**.

Review pipeline `test_docker`, then click **SAVE**.

<!-- EOF -->
