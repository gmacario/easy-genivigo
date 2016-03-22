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

Browse `${GOCD_URL}` (example: http://192.168.99.100:8153)

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
      /bin/sh
      ```

    - Arguments:

      ```
      -xe
      -c
      pwd; ls -la; ls -la c-poc
      ```

    - Working Directory: (none)

Click **FINISH**.

Review pipeline `build_easygenivigo`, then click **SAVE**.

### Run pipeline `build_capi_native`

Browse `${GOCD_URL}`, then click **PIPELINES**.

* Start pipeline `build_capi_native`.

Wait until pipeline `build_capi_native` finishes, then review log.

```
09:45:47.248 [go] Job Started: 2016-03-22 09:45:47 UTC

09:45:47.248 [go] Start to prepare build_capi_native/14/defaultStage/1/defaultJob on 0f29bb7cc96f [/var/lib/go-agent]
09:45:47.269 [go] Start to update materials.

09:45:47.269 [go] Start updating files at revision d6ec42ce45c33f40560d1f24b9143e9b1e6816e9 from git://git.projects.genivi.org/common-api/c-poc.git
09:45:47.383 [GIT] Fetch and reset in working directory pipelines/build_capi_native
09:45:47.383 [GIT] Cleaning all unversioned files in working copy
09:45:47.394 [GIT] Cleaning submodule configurations in .git/config
09:45:47.398 [GIT] Fetching changes
09:45:47.941 [GIT] Performing git gc
09:45:47.944 [GIT] Updating working copy to revision d6ec42ce45c33f40560d1f24b9143e9b1e6816e9
09:45:47.951 HEAD is now at d6ec42c release version v0.2.0
09:45:48.052 [GIT] Removing modified files in submodules
09:45:48.112 [GIT] Cleaning all unversioned files in working copy
09:45:48.251 [go] Done.

09:45:48.252 [go] setting environment variable 'GO_SERVER_URL' to value 'https://goserver:8154/go/'
09:45:48.252 [go] setting environment variable 'GO_TRIGGER_USER' to value 'anonymous'
09:45:48.252 [go] setting environment variable 'GO_PIPELINE_NAME' to value 'build_capi_native'
09:45:48.252 [go] setting environment variable 'GO_PIPELINE_COUNTER' to value '14'
09:45:48.252 [go] setting environment variable 'GO_PIPELINE_LABEL' to value '14'
09:45:48.253 [go] setting environment variable 'GO_STAGE_NAME' to value 'defaultStage'
09:45:48.253 [go] setting environment variable 'GO_STAGE_COUNTER' to value '1'
09:45:48.253 [go] setting environment variable 'GO_JOB_NAME' to value 'defaultJob'
09:45:48.253 [go] setting environment variable 'GO_REVISION' to value 'd6ec42ce45c33f40560d1f24b9143e9b1e6816e9'
09:45:48.253 [go] setting environment variable 'GO_TO_REVISION' to value 'd6ec42ce45c33f40560d1f24b9143e9b1e6816e9'
09:45:48.253 [go] setting environment variable 'GO_FROM_REVISION' to value 'd6ec42ce45c33f40560d1f24b9143e9b1e6816e9'

09:45:48.281 [go] Start to build build_capi_native/14/defaultStage/1/defaultJob on 0f29bb7cc96f [/var/lib/go-agent]
09:45:48.281 [go] Current job status: passed.

09:45:48.281 [go] Start to execute task: <exec command="sh" >
<arg>-c</arg>
<arg>id && printenv | sort && pwd && ls -la</arg>
</exec>.
09:45:48.289 uid=103(go) gid=106(go) groups=106(go),100(users),999(docker)
09:45:48.295 AGENT_STARTUP_ARGS=-Dcruise.console.publish.interval=10 -Xms128m -Xmx256m    -Djava.security.egd=file:/dev/./urandom
09:45:48.296 AGENT_WORK_DIR=/var/lib/go-agent
09:45:48.296 GO_FROM_REVISION=d6ec42ce45c33f40560d1f24b9143e9b1e6816e9
09:45:48.296 GO_JOB_NAME=defaultJob
09:45:48.296 GO_PIPELINE_COUNTER=14
09:45:48.297 GO_PIPELINE_LABEL=14
09:45:48.297 GO_PIPELINE_NAME=build_capi_native
09:45:48.297 GO_REVISION=d6ec42ce45c33f40560d1f24b9143e9b1e6816e9
09:45:48.298 GO_SERVER=goserver
09:45:48.298 GO_SERVER_PORT=8153
09:45:48.298 GO_SERVER_URL=https://goserver:8154/go/
09:45:48.299 GO_STAGE_COUNTER=1
09:45:48.299 GO_STAGE_NAME=defaultStage
09:45:48.299 GO_TO_REVISION=d6ec42ce45c33f40560d1f24b9143e9b1e6816e9
09:45:48.299 GO_TRIGGER_USER=anonymous
09:45:48.300 HOME=/var/go
09:45:48.300 HOSTNAME=0f29bb7cc96f
09:45:48.300 INITRD=no
09:45:48.301 JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/jre
09:45:48.301 LANG=en_US.UTF-8
09:45:48.301 LC_CTYPE=en_US.UTF-8
09:45:48.302 LOG_DIR=/var/log/go-agent
09:45:48.302 OLDPWD=/etc/service/go-agent
09:45:48.302 PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
09:45:48.302 PWD=/var/lib/go-agent/pipelines/build_capi_native
09:45:48.303 SHLVL=2
09:45:48.303 UID=103
09:45:48.303 USER=go
09:45:48.304 /var/lib/go-agent/pipelines/build_capi_native
09:45:48.308 total 92
09:45:48.308 drwxr-xr-x  8 go go  4096 Mar 22 09:45 .
09:45:48.309 drwxr-xr-x  5 go go  4096 Mar 21 16:15 ..
09:45:48.309 -rw-r--r--  1 go go   235 Mar 21 16:15 capic.pc.in
09:45:48.310 -rw-r--r--  1 go go  1228 Mar 21 16:15 configure.ac
09:45:48.310 drwxr-xr-x  2 go go  4096 Mar 21 16:15 doc
09:45:48.310 drwxr-xr-x  8 go go  4096 Mar 22 09:45 .git
09:45:48.311 -rw-r--r--  1 go go   199 Mar 21 16:15 .gitignore
09:45:48.311 -rw-r--r--  1 go go 11250 Mar 21 16:15 LICENSE.EPL-1.0
09:45:48.311 -rw-r--r--  1 go go 16726 Mar 21 16:15 LICENSE.MPL-2.0
09:45:48.312 -rw-r--r--  1 go go   870 Mar 21 16:15 Makefile.am
09:45:48.312 -rw-r--r--  1 go go  3019 Mar 21 16:15 NEWS
09:45:48.312 -rw-r--r--  1 go go  5758 Mar 21 16:15 README.adoc
09:45:48.312 drwxr-xr-x  5 go go  4096 Mar 21 16:15 ref
09:45:48.313 drwxr-xr-x  3 go go  4096 Mar 21 16:15 src
09:45:48.313 drwxr-xr-x  2 go go  4096 Mar 21 16:15 test
09:45:48.313 drwxr-xr-x 13 go go  4096 Mar 21 16:15 tools
09:45:48.409 [go] Current job status: passed.

09:45:48.409 [go] Start to execute task: <exec command="docker" >
<arg>pull</arg>
<arg>gmacario/build-capi-native</arg>
</exec>.
09:45:48.512 Using default tag: latest
09:45:51.069 latest: Pulling from gmacario/build-capi-native
09:45:51.070 6d3a6d998241: Already exists
09:45:51.071 606b08bdd0f3: Already exists
09:45:51.071 1d99b95ffc1c: Already exists
09:45:51.072 a3ed95caeb02: Already exists
09:45:51.072 64cb32515d1f: Already exists
09:45:51.072 27f758fbdce5: Already exists
09:45:51.073 cb86b15f160f: Already exists
09:45:51.073 7e6e2d8a0b33: Already exists
09:45:51.073 fde8067b0a52: Already exists
09:45:51.074 Digest: sha256:00dbd41b31d51f88be393a722009ac16ee21e71de401907128abc80ba1a83997
09:45:51.074 Status: Image is up to date for gmacario/build-capi-native:latest
09:45:51.077 [go] Current job status: passed.

09:45:51.077 [go] Start to execute task: <exec command="docker" >
<arg>run</arg>
<arg>-u</arg>
<arg>build</arg>
<arg>-w</arg>
<arg>/home/build</arg>
<arg>gmacario/build-capi-native</arg>
<arg>/bin/bash</arg>
<arg>-c</arg>
<arg>git clone git://git.projects.genivi.org/common-api/c-poc.git && cd c-poc && autoreconf -i && ./configure && make && sudo make install</arg>
</exec>.
09:45:51.401 Cloning into 'c-poc'...
09:45:56.755 libtoolize: putting auxiliary files in '.'.
09:45:56.755 libtoolize: copying file './ltmain.sh'
09:45:57.013 libtoolize: Consider adding 'AC_CONFIG_MACRO_DIRS([m4])' to configure.ac,
09:45:57.013 libtoolize: and rerunning libtoolize and aclocal.
09:45:57.013 libtoolize: Consider adding '-I m4' to ACLOCAL_AMFLAGS in Makefile.am.
09:45:59.828 configure.ac:18: installing './ar-lib'
09:45:59.833 configure.ac:17: installing './compile'
09:45:59.837 configure.ac:21: installing './config.guess'
09:45:59.841 configure.ac:21: installing './config.sub'
09:45:59.845 configure.ac:16: installing './install-sh'
09:45:59.850 configure.ac:16: installing './missing'
09:45:59.882 Makefile.am: installing './depcomp'
09:46:00.166 checking for a BSD-compatible install... /usr/bin/install -c
09:46:00.173 checking whether build environment is sane... yes
09:46:00.192 checking for a thread-safe mkdir -p... /bin/mkdir -p
09:46:00.193 checking for gawk... no
09:46:00.193 checking for mawk... mawk
09:46:00.213 checking whether make sets $(MAKE)... yes
09:46:00.226 checking whether make supports nested variables... yes
09:46:00.241 checking for gcc... gcc
09:46:00.399 checking whether the C compiler works... yes
09:46:00.399 checking for C compiler default output file name... a.out
09:46:00.466 checking for suffix of executables...
09:46:00.545 checking whether we are cross compiling... no
09:46:00.592 checking for suffix of object files... o
09:46:00.634 checking whether we are using the GNU C compiler... yes
09:46:00.676 checking whether gcc accepts -g... yes
09:46:00.747 checking for gcc option to accept ISO C89... none needed
09:46:00.821 checking whether gcc understands -c and -o together... yes
09:46:00.831 checking for style of include used by make... GNU
09:46:00.904 checking dependency style of gcc... gcc3
09:46:00.906 checking for ar... ar
09:46:00.953 checking the archiver (ar) interface... ar
09:46:01.073 checking build system type... x86_64-pc-linux-gnu
09:46:01.073 checking host system type... x86_64-pc-linux-gnu
09:46:01.079 checking how to print strings... printf
09:46:01.088 checking for a sed that does not truncate output... /bin/sed
09:46:01.096 checking for grep that handles long lines and -e... /bin/grep
09:46:01.100 checking for egrep... /bin/grep -E
09:46:01.104 checking for fgrep... /bin/grep -F
09:46:01.114 checking for ld used by gcc... /usr/bin/ld
09:46:01.119 checking if the linker (/usr/bin/ld) is GNU ld... yes
09:46:01.138 checking for BSD- or MS-compatible name lister (nm)... /usr/bin/nm -B
09:46:01.184 checking the name lister (/usr/bin/nm -B) interface... BSD nm
09:46:01.184 checking whether ln -s works... yes
09:46:01.193 checking the maximum length of command line arguments... 1572864
09:46:01.201 checking how to convert x86_64-pc-linux-gnu file names to x86_64-pc-linux-gnu format... func_convert_file_noop
09:46:01.202 checking how to convert x86_64-pc-linux-gnu file names to toolchain format... func_convert_file_noop
09:46:01.203 checking for /usr/bin/ld option to reload object files... -r
09:46:01.205 checking for objdump... objdump
09:46:01.206 checking how to recognize dependent libraries... pass_all
09:46:01.208 checking for dlltool... no
09:46:01.209 checking how to associate runtime and link libraries... printf %s\n
09:46:01.267 checking for archiver @FILE support... @
09:46:01.269 checking for strip... strip
09:46:01.270 checking for ranlib... ranlib
09:46:01.419 checking command to parse /usr/bin/nm -B output from gcc object... ok
09:46:01.431 checking for sysroot... no
09:46:01.439 checking for a working dd... /bin/dd
09:46:01.452 checking how to truncate binary pipes... /bin/dd bs=4096 count=1
09:46:01.493 checking for mt... no
09:46:01.499 checking if : is a manifest tool... no
09:46:01.551 checking how to run the C preprocessor... gcc -E
09:46:01.821 checking for ANSI C header files... yes
09:46:01.887 checking for sys/types.h... yes
09:46:01.963 checking for sys/stat.h... yes
09:46:02.039 checking for stdlib.h... yes
09:46:02.121 checking for string.h... yes
09:46:02.204 checking for memory.h... yes
09:46:02.287 checking for strings.h... yes
09:46:02.373 checking for inttypes.h... yes
09:46:02.458 checking for stdint.h... yes
09:46:02.551 checking for unistd.h... yes
09:46:02.638 checking for dlfcn.h... yes
09:46:02.649 checking for objdir... .libs
09:46:02.831 checking if gcc supports -fno-rtti -fno-exceptions... no
09:46:02.832 checking for gcc option to produce PIC... -fPIC -DPIC
09:46:02.879 checking if gcc PIC flag -fPIC -DPIC works... yes
09:46:03.016 checking if gcc static flag -static works... yes
09:46:03.079 checking if gcc supports -c -o file.o... yes
09:46:03.079 checking if gcc supports -c -o file.o... (cached) yes
09:46:03.097 checking whether the gcc linker (/usr/bin/ld -m elf_x86_64) supports shared libraries... yes
09:46:03.164 checking whether -lc should be explicitly linked in... no
09:46:03.278 checking dynamic linker characteristics... GNU/Linux ld.so
09:46:03.278 checking how to hardcode library paths into programs... immediate
09:46:03.284 checking whether stripping libraries is possible... yes
09:46:03.285 checking if libtool supports shared libraries... yes
09:46:03.285 checking whether to build shared libraries... yes
09:46:03.286 checking whether to build static libraries... no
09:46:03.289 checking for pkg-config... /usr/bin/pkg-config
09:46:03.293 checking pkg-config is at least version 0.9.0... yes
09:46:03.311 checking for LIBSYSTEMD... yes
09:46:03.398 checking for sd_bus_open in -lsystemd... yes
09:46:03.485 checking for sd_bus_get_scope in -lsystemd... yes
09:46:03.569 checking whether SD_EVENT_INITIAL is declared... yes
09:46:03.626 checking that generated files are newer than configure... done
09:46:03.627 configure: creating ./config.status
09:46:04.585 config.status: creating Makefile
09:46:04.613 config.status: creating capic.pc
09:46:04.640 config.status: creating config.h
09:46:04.659 config.status: executing depfiles commands
09:46:04.703 config.status: executing libtool commands
09:46:04.801 make  all-am
09:46:04.809 make[1]: Entering directory '/home/build/c-poc'
09:46:04.825 /bin/bash ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.    -I./src -I.  -Wall -Werror -fvisibility=hidden -g -O2 -MT src/libcapic_la-backend.lo -MD -MP -MF src/.deps/libcapic_la-backend.Tpo -c -o src/libcapic_la-backend.lo `test -f 'src/backend.c' || echo './'`src/backend.c
09:46:04.932 libtool: compile:  gcc -DHAVE_CONFIG_H -I. -I./src -I. -Wall -Werror -fvisibility=hidden -g -O2 -MT src/libcapic_la-backend.lo -MD -MP -MF src/.deps/libcapic_la-backend.Tpo -c src/backend.c  -fPIC -DPIC -o src/.libs/libcapic_la-backend.o
09:46:05.126 mv -f src/.deps/libcapic_la-backend.Tpo src/.deps/libcapic_la-backend.Plo
09:46:05.132 /bin/bash ./libtool  --tag=CC   --mode=link gcc -I./src -I.  -Wall -Werror -fvisibility=hidden -g -O2 -no-undefined -version-info 0:0:0  -o libcapic.la -rpath /usr/local/lib  src/libcapic_la-backend.lo  
09:46:05.299 libtool: link: gcc -shared  -fPIC -DPIC  src/.libs/libcapic_la-backend.o    -g -O2   -Wl,-soname -Wl,libcapic.so.0 -o .libs/libcapic.so.0.0.0
09:46:05.342 libtool: link: (cd ".libs" && rm -f "libcapic.so.0" && ln -s "libcapic.so.0.0.0" "libcapic.so.0")
09:46:05.355 libtool: link: (cd ".libs" && rm -f "libcapic.so" && ln -s "libcapic.so.0.0.0" "libcapic.so")
09:46:05.380 libtool: link: ( cd ".libs" && rm -f "libcapic.la" && ln -s "../libcapic.la" "libcapic.la" )
09:46:05.394 make[1]: Leaving directory '/home/build/c-poc'
09:46:05.509 make[1]: Entering directory '/home/build/c-poc'
09:46:05.516  /bin/mkdir -p '/usr/local/lib'
09:46:05.519  /bin/bash ./libtool   --mode=install /usr/bin/install -c   libcapic.la '/usr/local/lib'
09:46:05.570 libtool: install: /usr/bin/install -c .libs/libcapic.so.0.0.0 /usr/local/lib/libcapic.so.0.0.0
09:46:05.575 libtool: install: (cd /usr/local/lib && { ln -s -f libcapic.so.0.0.0 libcapic.so.0 || { rm -f libcapic.so.0 && ln -s libcapic.so.0.0.0 libcapic.so.0; }; })
09:46:05.579 libtool: install: (cd /usr/local/lib && { ln -s -f libcapic.so.0.0.0 libcapic.so || { rm -f libcapic.so && ln -s libcapic.so.0.0.0 libcapic.so; }; })
09:46:05.583 libtool: install: /usr/bin/install -c .libs/libcapic.lai /usr/local/lib/libcapic.la
09:46:05.666 libtool: finish: PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/sbin" ldconfig -n /usr/local/lib
09:46:05.671 ----------------------------------------------------------------------
09:46:05.672 Libraries have been installed in:
09:46:05.672    /usr/local/lib
09:46:05.672
09:46:05.673 If you ever happen to want to link against installed libraries
09:46:05.673 in a given directory, LIBDIR, you must either use libtool, and
09:46:05.674 specify the full pathname of the library, or use the '-LLIBDIR'
09:46:05.674 flag during linking and do at least one of the following:
09:46:05.675    - add LIBDIR to the 'LD_LIBRARY_PATH' environment variable
09:46:05.675      during execution
09:46:05.676    - add LIBDIR to the 'LD_RUN_PATH' environment variable
09:46:05.676      during linking
09:46:05.676    - use the '-Wl,-rpath -Wl,LIBDIR' linker flag
09:46:05.677    - have your system administrator add LIBDIR to '/etc/ld.so.conf'
09:46:05.677
09:46:05.677 See any operating system documentation about shared libraries for
09:46:05.680 more information, such as the ld(1) and ld.so(8) manual pages.
09:46:05.680 ----------------------------------------------------------------------
09:46:05.681  /bin/mkdir -p '/usr/local/lib/pkgconfig'
09:46:05.682  /usr/bin/install -c -m 644 capic.pc '/usr/local/lib/pkgconfig'
09:46:05.688  /bin/mkdir -p '/usr/local/include/capic'
09:46:05.695  /usr/bin/install -c -m 644 src/capic/backend.h src/capic/log.h src/capic/dbus-private.h '/usr/local/include/capic'
09:46:05.700 make[1]: Leaving directory '/home/build/c-poc'
09:46:06.008 [go] Current job status: passed.

09:46:06.008 [go] Start to execute task: <exec command="sh" >
<arg>-c</arg>
<arg>pwd</arg>
<arg>ls -la</arg>
<arg>ls -la c-poc</arg>
</exec>.
09:46:06.013 /var/lib/go-agent/pipelines/build_capi_native
09:46:06.140 [go] Current job status: passed.

09:46:06.156 [go] Start to create properties build_capi_native/14/defaultStage/1/defaultJob on 0f29bb7cc96f [/var/lib/go-agent]
09:46:06.156 [go] Start to upload build_capi_native/14/defaultStage/1/defaultJob on 0f29bb7cc96f [/var/lib/go-agent]
09:46:06.323 [go] Job completed build_capi_native/14/defaultStage/1/defaultJob on 0f29bb7cc96f [/var/lib/go-agent]
```

<!-- EOF -->
