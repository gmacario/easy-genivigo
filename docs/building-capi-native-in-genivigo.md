Building Common API C in easy-genivigo
======================================

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

### Create pipeline `build_capi_native`

Browse `${GOCD_URL}`, then click **Admin** > **Pipelines**.

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
      /bin/sh
      ```

    - Arguments:

      ```
      -xe
      -c
      id; printenv | sort; pwd; ls -la
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
      --rm
      --user
      build
      --workdir
      /home/build
      gmacario/build-capi-native
      /bin/bash
      -xe
      -c
      git clone git://git.projects.genivi.org/common-api/c-poc.git && cd c-poc && autoreconf -i && ./configure && make && sudo make install
      ```

    - Working Directory: (none)

Click **FINISH**.

Review pipeline `build_easygenivigo`, then click **SAVE**.

### Run pipeline `build_capi_native`

Browse `${GOCD_URL}`, then click **PIPELINES**.

* Start pipeline `build_capi_native`.

Wait until pipeline `build_capi_native` finishes, then review log.

```
10:34:20.087 [go] Job Started: 2016-03-22 10:34:20 UTC

10:34:20.088 [go] Start to prepare build_capi_native/2/defaultStage/1/defaultJob on c19594724e15 [/var/lib/go-agent]
10:34:20.116 [go] Start to update materials.

10:34:20.116 [go] Start updating files at revision d6ec42ce45c33f40560d1f24b9143e9b1e6816e9 from git://git.projects.genivi.org/common-api/c-poc.git
10:34:20.228 [GIT] Fetch and reset in working directory pipelines/build_capi_native
10:34:20.228 [GIT] Cleaning all unversioned files in working copy
10:34:20.340 [GIT] Cleaning submodule configurations in .git/config
10:34:20.347 [GIT] Fetching changes
10:34:20.857 [GIT] Performing git gc
10:34:20.861 [GIT] Updating working copy to revision d6ec42ce45c33f40560d1f24b9143e9b1e6816e9
10:34:20.870 HEAD is now at d6ec42c release version v0.2.0
10:34:20.970 [GIT] Removing modified files in submodules
10:34:21.009 [GIT] Cleaning all unversioned files in working copy
10:34:21.020 [go] Done.

10:34:21.024 [go] setting environment variable 'GO_SERVER_URL' to value 'https://goserver:8154/go/'
10:34:21.024 [go] setting environment variable 'GO_TRIGGER_USER' to value 'anonymous'
10:34:21.024 [go] setting environment variable 'GO_PIPELINE_NAME' to value 'build_capi_native'
10:34:21.024 [go] setting environment variable 'GO_PIPELINE_COUNTER' to value '2'
10:34:21.024 [go] setting environment variable 'GO_PIPELINE_LABEL' to value '2'
10:34:21.024 [go] setting environment variable 'GO_STAGE_NAME' to value 'defaultStage'
10:34:21.025 [go] setting environment variable 'GO_STAGE_COUNTER' to value '1'
10:34:21.025 [go] setting environment variable 'GO_JOB_NAME' to value 'defaultJob'
10:34:21.025 [go] setting environment variable 'GO_REVISION' to value 'd6ec42ce45c33f40560d1f24b9143e9b1e6816e9'
10:34:21.025 [go] setting environment variable 'GO_TO_REVISION' to value 'd6ec42ce45c33f40560d1f24b9143e9b1e6816e9'
10:34:21.025 [go] setting environment variable 'GO_FROM_REVISION' to value 'd6ec42ce45c33f40560d1f24b9143e9b1e6816e9'

10:34:21.066 [go] Start to build build_capi_native/2/defaultStage/1/defaultJob on c19594724e15 [/var/lib/go-agent]
10:34:21.067 [go] Current job status: passed.

10:34:21.067 [go] Start to execute task: <exec command="/bin/sh" >
<arg>-xe</arg>
<arg>-c</arg>
<arg>id; printenv | sort; pwd; ls -la</arg>
</exec>.
10:34:21.088 + id
10:34:21.089 + sort
10:34:21.089 uid=103(go) gid=106(go) groups=106(go),100(users),999(docker)
10:34:21.091 + printenv
10:34:21.093 AGENT_STARTUP_ARGS=-Dcruise.console.publish.interval=10 -Xms128m -Xmx256m    -Djava.security.egd=file:/dev/./urandom
10:34:21.093 AGENT_WORK_DIR=/var/lib/go-agent
10:34:21.094 GO_FROM_REVISION=d6ec42ce45c33f40560d1f24b9143e9b1e6816e9
10:34:21.094 GO_JOB_NAME=defaultJob
10:34:21.095 + pwd
10:34:21.095 GO_PIPELINE_COUNTER=2
10:34:21.096 + ls -la
10:34:21.096 GO_PIPELINE_LABEL=2
10:34:21.097 GO_PIPELINE_NAME=build_capi_native
10:34:21.098 GO_REVISION=d6ec42ce45c33f40560d1f24b9143e9b1e6816e9
10:34:21.098 GO_SERVER=goserver
10:34:21.099 GO_SERVER_PORT=8153
10:34:21.099 GO_SERVER_URL=https://goserver:8154/go/
10:34:21.102 GO_STAGE_COUNTER=1
10:34:21.103 GO_STAGE_NAME=defaultStage
10:34:21.104 GO_TO_REVISION=d6ec42ce45c33f40560d1f24b9143e9b1e6816e9
10:34:21.104 GO_TRIGGER_USER=anonymous
10:34:21.105 HOME=/var/go
10:34:21.105 HOSTNAME=c19594724e15
10:34:21.106 INITRD=no
10:34:21.106 JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/jre
10:34:21.107 LANG=en_US.UTF-8
10:34:21.108 LC_CTYPE=en_US.UTF-8
10:34:21.108 LOG_DIR=/var/log/go-agent
10:34:21.109 OLDPWD=/etc/service/go-agent
10:34:21.109 PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
10:34:21.110 PWD=/var/lib/go-agent/pipelines/build_capi_native
10:34:21.111 SHLVL=2
10:34:21.111 UID=103
10:34:21.112 USER=go
10:34:21.112 /var/lib/go-agent/pipelines/build_capi_native
10:34:21.113 total 92
10:34:21.113 drwxr-xr-x  8 go go  4096 Mar 22 10:34 .
10:34:21.114 drwxr-xr-x  3 go go  4096 Mar 22 10:31 ..
10:34:21.115 -rw-r--r--  1 go go   235 Mar 22 10:31 capic.pc.in
10:34:21.115 -rw-r--r--  1 go go  1228 Mar 22 10:31 configure.ac
10:34:21.116 drwxr-xr-x  2 go go  4096 Mar 22 10:31 doc
10:34:21.116 drwxr-xr-x  8 go go  4096 Mar 22 10:34 .git
10:34:21.117 -rw-r--r--  1 go go   199 Mar 22 10:31 .gitignore
10:34:21.118 -rw-r--r--  1 go go 11250 Mar 22 10:31 LICENSE.EPL-1.0
10:34:21.118 -rw-r--r--  1 go go 16726 Mar 22 10:31 LICENSE.MPL-2.0
10:34:21.119 -rw-r--r--  1 go go   870 Mar 22 10:31 Makefile.am
10:34:21.119 -rw-r--r--  1 go go  3019 Mar 22 10:31 NEWS
10:34:21.120 -rw-r--r--  1 go go  5758 Mar 22 10:31 README.adoc
10:34:21.121 drwxr-xr-x  5 go go  4096 Mar 22 10:31 ref
10:34:21.121 drwxr-xr-x  3 go go  4096 Mar 22 10:31 src
10:34:21.122 drwxr-xr-x  2 go go  4096 Mar 22 10:31 test
10:34:21.123 drwxr-xr-x 13 go go  4096 Mar 22 10:31 tools
10:34:21.201 [go] Current job status: passed.

10:34:21.201 [go] Start to execute task: <exec command="docker" >
<arg>pull</arg>
<arg>gmacario/build-capi-native</arg>
</exec>.
10:34:21.282 Using default tag: latest
10:34:24.144 latest: Pulling from gmacario/build-capi-native
10:34:24.146 6d3a6d998241: Already exists
10:34:24.152 606b08bdd0f3: Already exists
10:34:24.152 1d99b95ffc1c: Already exists
10:34:24.153 a3ed95caeb02: Already exists
10:34:24.153 64cb32515d1f: Already exists
10:34:24.154 27f758fbdce5: Already exists
10:34:24.155 cb86b15f160f: Already exists
10:34:24.155 7e6e2d8a0b33: Already exists
10:34:24.156 fde8067b0a52: Already exists
10:34:24.156 Digest: sha256:00dbd41b31d51f88be393a722009ac16ee21e71de401907128abc80ba1a83997
10:34:24.157 Status: Image is up to date for gmacario/build-capi-native:latest
10:34:24.250 [go] Current job status: passed.

10:34:24.251 [go] Start to execute task: <exec command="docker" >
<arg>run</arg>
<arg>--rm</arg>
<arg>--user</arg>
<arg>build</arg>
<arg>--workdir</arg>
<arg>/home/build</arg>
<arg>gmacario/build-capi-native</arg>
<arg>/bin/bash</arg>
<arg>-xe</arg>
<arg>-c</arg>
<arg>git clone git://git.projects.genivi.org/common-api/c-poc.git && cd c-poc && autoreconf -i && ./configure && make && sudo make install</arg>
</exec>.
10:34:24.570 + git clone git://git.projects.genivi.org/common-api/c-poc.git
10:34:24.578 Cloning into 'c-poc'...
10:34:26.182 + cd c-poc
10:34:26.183 + autoreconf -i
10:34:29.654 libtoolize: putting auxiliary files in '.'.
10:34:29.655 libtoolize: copying file './ltmain.sh'
10:34:29.921 libtoolize: Consider adding 'AC_CONFIG_MACRO_DIRS([m4])' to configure.ac,
10:34:29.922 libtoolize: and rerunning libtoolize and aclocal.
10:34:29.923 libtoolize: Consider adding '-I m4' to ACLOCAL_AMFLAGS in Makefile.am.
10:34:32.797 configure.ac:18: installing './ar-lib'
10:34:32.802 configure.ac:17: installing './compile'
10:34:32.807 configure.ac:21: installing './config.guess'
10:34:32.811 configure.ac:21: installing './config.sub'
10:34:32.816 configure.ac:16: installing './install-sh'
10:34:32.821 configure.ac:16: installing './missing'
10:34:32.853 Makefile.am: installing './depcomp'
10:34:32.944 + ./configure
10:34:33.147 checking for a BSD-compatible install... /usr/bin/install -c
10:34:33.154 checking whether build environment is sane... yes
10:34:33.174 checking for a thread-safe mkdir -p... /bin/mkdir -p
10:34:33.176 checking for gawk... no
10:34:33.177 checking for mawk... mawk
10:34:33.193 checking whether make sets $(MAKE)... yes
10:34:33.206 checking whether make supports nested variables... yes
10:34:33.222 checking for gcc... gcc
10:34:33.336 checking whether the C compiler works... yes
10:34:33.337 checking for C compiler default output file name... a.out
10:34:33.407 checking for suffix of executables...
10:34:33.489 checking whether we are cross compiling... no
10:34:33.536 checking for suffix of object files... o
10:34:33.580 checking whether we are using the GNU C compiler... yes
10:34:33.622 checking whether gcc accepts -g... yes
10:34:33.695 checking for gcc option to accept ISO C89... none needed
10:34:33.771 checking whether gcc understands -c and -o together... yes
10:34:33.782 checking for style of include used by make... GNU
10:34:33.859 checking dependency style of gcc... gcc3
10:34:33.861 checking for ar... ar
10:34:33.904 checking the archiver (ar) interface... ar
10:34:34.031 checking build system type... x86_64-pc-linux-gnu
10:34:34.032 checking host system type... x86_64-pc-linux-gnu
10:34:34.036 checking how to print strings... printf
10:34:34.052 checking for a sed that does not truncate output... /bin/sed
10:34:34.053 checking for grep that handles long lines and -e... /bin/grep
10:34:34.056 checking for egrep... /bin/grep -E
10:34:34.070 checking for fgrep... /bin/grep -F
10:34:34.080 checking for ld used by gcc... /usr/bin/ld
10:34:34.085 checking if the linker (/usr/bin/ld) is GNU ld... yes
10:34:34.090 checking for BSD- or MS-compatible name lister (nm)... /usr/bin/nm -B
10:34:34.138 checking the name lister (/usr/bin/nm -B) interface... BSD nm
10:34:34.138 checking whether ln -s works... yes
10:34:34.147 checking the maximum length of command line arguments... 1572864
10:34:34.155 checking how to convert x86_64-pc-linux-gnu file names to x86_64-pc-linux-gnu format... func_convert_file_noop
10:34:34.156 checking how to convert x86_64-pc-linux-gnu file names to toolchain format... func_convert_file_noop
10:34:34.157 checking for /usr/bin/ld option to reload object files... -r
10:34:34.158 checking for objdump... objdump
10:34:34.159 checking how to recognize dependent libraries... pass_all
10:34:34.160 checking for dlltool... no
10:34:34.161 checking how to associate runtime and link libraries... printf %s\n
10:34:34.221 checking for archiver @FILE support... @
10:34:34.223 checking for strip... strip
10:34:34.224 checking for ranlib... ranlib
10:34:34.370 checking command to parse /usr/bin/nm -B output from gcc object... ok
10:34:34.375 checking for sysroot... no
10:34:34.388 checking for a working dd... /bin/dd
10:34:34.397 checking how to truncate binary pipes... /bin/dd bs=4096 count=1
10:34:34.436 checking for mt... no
10:34:34.443 checking if : is a manifest tool... no
10:34:34.495 checking how to run the C preprocessor... gcc -E
10:34:34.765 checking for ANSI C header files... yes
10:34:34.831 checking for sys/types.h... yes
10:34:34.908 checking for sys/stat.h... yes
10:34:34.983 checking for stdlib.h... yes
10:34:35.066 checking for string.h... yes
10:34:35.149 checking for memory.h... yes
10:34:35.232 checking for strings.h... yes
10:34:35.318 checking for inttypes.h... yes
10:34:35.405 checking for stdint.h... yes
10:34:35.496 checking for unistd.h... yes
10:34:35.583 checking for dlfcn.h... yes
10:34:35.594 checking for objdir... .libs
10:34:35.773 checking if gcc supports -fno-rtti -fno-exceptions... no
10:34:35.774 checking for gcc option to produce PIC... -fPIC -DPIC
10:34:35.820 checking if gcc PIC flag -fPIC -DPIC works... yes
10:34:35.956 checking if gcc static flag -static works... yes
10:34:36.018 checking if gcc supports -c -o file.o... yes
10:34:36.019 checking if gcc supports -c -o file.o... (cached) yes
10:34:36.038 checking whether the gcc linker (/usr/bin/ld -m elf_x86_64) supports shared libraries... yes
10:34:36.104 checking whether -lc should be explicitly linked in... no
10:34:36.216 checking dynamic linker characteristics... GNU/Linux ld.so
10:34:36.218 checking how to hardcode library paths into programs... immediate
10:34:36.223 checking whether stripping libraries is possible... yes
10:34:36.224 checking if libtool supports shared libraries... yes
10:34:36.225 checking whether to build shared libraries... yes
10:34:36.226 checking whether to build static libraries... no
10:34:36.227 checking for pkg-config... /usr/bin/pkg-config
10:34:36.231 checking pkg-config is at least version 0.9.0... yes
10:34:36.249 checking for LIBSYSTEMD... yes
10:34:36.336 checking for sd_bus_open in -lsystemd... yes
10:34:36.422 checking for sd_bus_get_scope in -lsystemd... yes
10:34:36.509 checking whether SD_EVENT_INITIAL is declared... yes
10:34:36.564 checking that generated files are newer than configure... done
10:34:36.565 configure: creating ./config.status
10:34:37.499 config.status: creating Makefile
10:34:37.525 config.status: creating capic.pc
10:34:37.551 config.status: creating config.h
10:34:37.569 config.status: executing depfiles commands
10:34:37.612 config.status: executing libtool commands
10:34:37.706 + make
10:34:37.716 make  all-am
10:34:37.725 make[1]: Entering directory '/home/build/c-poc'
10:34:37.743 /bin/bash ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.    -I./src -I.  -Wall -Werror -fvisibility=hidden -g -O2 -MT src/libcapic_la-backend.lo -MD -MP -MF src/.deps/libcapic_la-backend.Tpo -c -o src/libcapic_la-backend.lo `test -f 'src/backend.c' || echo './'`src/backend.c
10:34:37.849 libtool: compile:  gcc -DHAVE_CONFIG_H -I. -I./src -I. -Wall -Werror -fvisibility=hidden -g -O2 -MT src/libcapic_la-backend.lo -MD -MP -MF src/.deps/libcapic_la-backend.Tpo -c src/backend.c  -fPIC -DPIC -o src/.libs/libcapic_la-backend.o
10:34:38.040 mv -f src/.deps/libcapic_la-backend.Tpo src/.deps/libcapic_la-backend.Plo
10:34:38.045 /bin/bash ./libtool  --tag=CC   --mode=link gcc -I./src -I.  -Wall -Werror -fvisibility=hidden -g -O2 -no-undefined -version-info 0:0:0  -o libcapic.la -rpath /usr/local/lib  src/libcapic_la-backend.lo  
10:34:38.210 libtool: link: gcc -shared  -fPIC -DPIC  src/.libs/libcapic_la-backend.o    -g -O2   -Wl,-soname -Wl,libcapic.so.0 -o .libs/libcapic.so.0.0.0
10:34:38.248 libtool: link: (cd ".libs" && rm -f "libcapic.so.0" && ln -s "libcapic.so.0.0.0" "libcapic.so.0")
10:34:38.261 libtool: link: (cd ".libs" && rm -f "libcapic.so" && ln -s "libcapic.so.0.0.0" "libcapic.so")
10:34:38.286 libtool: link: ( cd ".libs" && rm -f "libcapic.la" && ln -s "../libcapic.la" "libcapic.la" )
10:34:38.301 make[1]: Leaving directory '/home/build/c-poc'
10:34:38.303 + sudo make install
10:34:38.341 make[1]: Entering directory '/home/build/c-poc'
10:34:38.348  /bin/mkdir -p '/usr/local/lib'
10:34:38.350  /bin/bash ./libtool   --mode=install /usr/bin/install -c   libcapic.la '/usr/local/lib'
10:34:38.401 libtool: install: /usr/bin/install -c .libs/libcapic.so.0.0.0 /usr/local/lib/libcapic.so.0.0.0
10:34:38.406 libtool: install: (cd /usr/local/lib && { ln -s -f libcapic.so.0.0.0 libcapic.so.0 || { rm -f libcapic.so.0 && ln -s libcapic.so.0.0.0 libcapic.so.0; }; })
10:34:38.410 libtool: install: (cd /usr/local/lib && { ln -s -f libcapic.so.0.0.0 libcapic.so || { rm -f libcapic.so && ln -s libcapic.so.0.0.0 libcapic.so; }; })
10:34:38.414 libtool: install: /usr/bin/install -c .libs/libcapic.lai /usr/local/lib/libcapic.la
10:34:38.495 libtool: finish: PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/sbin" ldconfig -n /usr/local/lib
10:34:38.500 ----------------------------------------------------------------------
10:34:38.500 Libraries have been installed in:
10:34:38.501    /usr/local/lib
10:34:38.501
10:34:38.502 If you ever happen to want to link against installed libraries
10:34:38.502 in a given directory, LIBDIR, you must either use libtool, and
10:34:38.503 specify the full pathname of the library, or use the '-LLIBDIR'
10:34:38.506 flag during linking and do at least one of the following:
10:34:38.507    - add LIBDIR to the 'LD_LIBRARY_PATH' environment variable
10:34:38.508      during execution
10:34:38.508    - add LIBDIR to the 'LD_RUN_PATH' environment variable
10:34:38.509      during linking
10:34:38.509    - use the '-Wl,-rpath -Wl,LIBDIR' linker flag
10:34:38.510    - have your system administrator add LIBDIR to '/etc/ld.so.conf'
10:34:38.510
10:34:38.516 See any operating system documentation about shared libraries for
10:34:38.517 more information, such as the ld(1) and ld.so(8) manual pages.
10:34:38.517 ----------------------------------------------------------------------
10:34:38.518  /bin/mkdir -p '/usr/local/lib/pkgconfig'
10:34:38.522  /usr/bin/install -c -m 644 capic.pc '/usr/local/lib/pkgconfig'
10:34:38.526  /bin/mkdir -p '/usr/local/include/capic'
10:34:38.533  /usr/bin/install -c -m 644 src/capic/backend.h src/capic/log.h src/capic/dbus-private.h '/usr/local/include/capic'
10:34:38.537 make[1]: Leaving directory '/home/build/c-poc'
10:34:38.819 [go] Current job status: passed.

10:34:38.842 [go] Start to create properties build_capi_native/2/defaultStage/1/defaultJob on c19594724e15 [/var/lib/go-agent]
10:34:38.843 [go] Start to upload build_capi_native/2/defaultStage/1/defaultJob on c19594724e15 [/var/lib/go-agent]
10:34:39.172 [go] Job completed build_capi_native/2/defaultStage/1/defaultJob on c19594724e15 [/var/lib/go-agent]
```

<!-- EOF -->
