# Building easy-genivigo inside easy-jenkins

**WORK-IN-PROGRESS**

This document contains instructions for building [easy-genivigo](https://github.com/gmacario/easy-genivigo) inside [easy-jenkins](https://github.com/gmacario/easy-jenkins).

This procedure may be useful to verify non-regressions of a new feature/bugfix before merging it to the master branch.

## Preparation

* Install and configure easy-jenkins - please refer to [preparation.md](https://github.com/gmacario/easy-jenkins/blob/master/docs/preparation.md) for details.
* Verify that the Jenkins Dashboard is accessible at `${DOCKER_URL}` (example: http://192.168.99.100:9080/)

## Step-by-step instructions

### Configure project `build_easy_genivigo`

* Browse `${JENKINS_URL}`, then click **New Item**
  - Item name: `build_easy_genivigo`
  - Type: **Freestyle project**

  then click **OK**.

**TODO**: Configure a Pipeline instead than a Freestyle project

* Inside the project configuration page, fill-in the following information:
  - Discard Old Builds: Yes
    - Strategy: Log Rotation
      - Days to keep builds: (none)
      - Max # of builds to keep: 5
  - Restrict where this project can be run: `master` (TODO: Define and use label `docker`)
  - Source Code Management: Git
    - Repositories
      - Repository URL: `https://github.com/gmacario/easy-genivigo`
      - Credentials: - none -
      - Branches to build
        - Branch Specifier (blank for `any`): `*/master`
      - Repository browser: (Auto)
  - Build
    - Add build step > Execute shell
      - Command

```
#!/bin/bash -xe

docker-compose build --pull
./runme.sh

# EOF
```

  then click **Save**

### Build project `build_easy_genivigo
<!-- (2016-02-24 12:50 CET) -->

* Browse `${JENKINS_URL}/job/build_easy_genivigo`, then click **Build Now**

Result: TODO

```
TODO
```

<!-- EOF -->
