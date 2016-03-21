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
17:45:07.452 [go] Job Started: 2016-03-21 17:45:07 UTC

17:45:07.453 [go] Start to prepare build_capi_native/13/defaultStage/1/defaultJob on 0f29bb7cc96f [/var/lib/go-agent]
17:45:07.472 [go] Start to update materials.

17:45:07.473 [go] Start updating files at revision d6ec42ce45c33f40560d1f24b9143e9b1e6816e9 from git://git.projects.genivi.org/common-api/c-poc.git
17:45:07.481 [GIT] Fetch and reset in working directory pipelines/build_capi_native
17:45:07.481 [GIT] Cleaning all unversioned files in working copy
17:45:07.492 [GIT] Cleaning submodule configurations in .git/config
17:45:07.595 [GIT] Fetching changes
17:45:08.176 [GIT] Performing git gc
17:45:08.282 [GIT] Updating working copy to revision d6ec42ce45c33f40560d1f24b9143e9b1e6816e9
17:45:08.289 HEAD is now at d6ec42c release version v0.2.0
17:45:08.390 [GIT] Removing modified files in submodules
17:45:08.426 [GIT] Cleaning all unversioned files in working copy
17:45:08.540 [go] Done.

17:45:08.541 [go] setting environment variable 'GO_SERVER_URL' to value 'https://goserver:8154/go/'
17:45:08.541 [go] setting environment variable 'GO_TRIGGER_USER' to value 'anonymous'
17:45:08.541 [go] setting environment variable 'GO_PIPELINE_NAME' to value 'build_capi_native'
17:45:08.541 [go] setting environment variable 'GO_PIPELINE_COUNTER' to value '13'
17:45:08.541 [go] setting environment variable 'GO_PIPELINE_LABEL' to value '13'
17:45:08.541 [go] setting environment variable 'GO_STAGE_NAME' to value 'defaultStage'
17:45:08.542 [go] setting environment variable 'GO_STAGE_COUNTER' to value '1'
17:45:08.542 [go] setting environment variable 'GO_JOB_NAME' to value 'defaultJob'
17:45:08.542 [go] setting environment variable 'GO_REVISION' to value 'd6ec42ce45c33f40560d1f24b9143e9b1e6816e9'
17:45:08.542 [go] setting environment variable 'GO_TO_REVISION' to value 'd6ec42ce45c33f40560d1f24b9143e9b1e6816e9'
17:45:08.542 [go] setting environment variable 'GO_FROM_REVISION' to value 'd6ec42ce45c33f40560d1f24b9143e9b1e6816e9'

17:45:08.572 [go] Start to build build_capi_native/13/defaultStage/1/defaultJob on 0f29bb7cc96f [/var/lib/go-agent]
17:45:08.573 [go] Current job status: passed.

17:45:08.573 [go] Start to execute task: <exec command="sh" >
<arg>-c</arg>
<arg>id && printenv | sort && pwd && ls -la</arg>
</exec>.
17:45:08.584 uid=103(go) gid=106(go) groups=106(go),100(users),999(docker)
17:45:08.586 AGENT_STARTUP_ARGS=-Dcruise.console.publish.interval=10 -Xms128m -Xmx256m    -Djava.security.egd=file:/dev/./urandom
17:45:08.586 AGENT_WORK_DIR=/var/lib/go-agent
17:45:08.587 GO_FROM_REVISION=d6ec42ce45c33f40560d1f24b9143e9b1e6816e9
17:45:08.587 GO_JOB_NAME=defaultJob
17:45:08.587 GO_PIPELINE_COUNTER=13
17:45:08.588 GO_PIPELINE_LABEL=13
17:45:08.588 GO_PIPELINE_NAME=build_capi_native
17:45:08.588 GO_REVISION=d6ec42ce45c33f40560d1f24b9143e9b1e6816e9
17:45:08.589 GO_SERVER=goserver
17:45:08.589 GO_SERVER_PORT=8153
17:45:08.589 GO_SERVER_URL=https://goserver:8154/go/
17:45:08.590 GO_STAGE_COUNTER=1
17:45:08.590 GO_STAGE_NAME=defaultStage
17:45:08.591 GO_TO_REVISION=d6ec42ce45c33f40560d1f24b9143e9b1e6816e9
17:45:08.594 GO_TRIGGER_USER=anonymous
17:45:08.594 HOME=/var/go
17:45:08.595 HOSTNAME=0f29bb7cc96f
17:45:08.595 INITRD=no
17:45:08.595 JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/jre
17:45:08.596 LANG=en_US.UTF-8
17:45:08.596 LC_CTYPE=en_US.UTF-8
17:45:08.596 LOG_DIR=/var/log/go-agent
17:45:08.596 OLDPWD=/etc/service/go-agent
17:45:08.597 PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
17:45:08.597 PWD=/var/lib/go-agent/pipelines/build_capi_native
17:45:08.597 SHLVL=2
17:45:08.598 UID=103
17:45:08.598 USER=go
17:45:08.598 /var/lib/go-agent/pipelines/build_capi_native
17:45:08.598 total 92
17:45:08.599 drwxr-xr-x  8 go go  4096 Mar 21 17:45 .
17:45:08.599 drwxr-xr-x  5 go go  4096 Mar 21 16:15 ..
17:45:08.599 -rw-r--r--  1 go go   235 Mar 21 16:15 capic.pc.in
17:45:08.599 -rw-r--r--  1 go go  1228 Mar 21 16:15 configure.ac
17:45:08.600 drwxr-xr-x  2 go go  4096 Mar 21 16:15 doc
17:45:08.600 drwxr-xr-x  8 go go  4096 Mar 21 17:45 .git
17:45:08.600 -rw-r--r--  1 go go   199 Mar 21 16:15 .gitignore
17:45:08.601 -rw-r--r--  1 go go 11250 Mar 21 16:15 LICENSE.EPL-1.0
17:45:08.601 -rw-r--r--  1 go go 16726 Mar 21 16:15 LICENSE.MPL-2.0
17:45:08.601 -rw-r--r--  1 go go   870 Mar 21 16:15 Makefile.am
17:45:08.601 -rw-r--r--  1 go go  3019 Mar 21 16:15 NEWS
17:45:08.602 -rw-r--r--  1 go go  5758 Mar 21 16:15 README.adoc
17:45:08.602 drwxr-xr-x  5 go go  4096 Mar 21 16:15 ref
17:45:08.602 drwxr-xr-x  3 go go  4096 Mar 21 16:15 src
17:45:08.602 drwxr-xr-x  2 go go  4096 Mar 21 16:15 test
17:45:08.603 drwxr-xr-x 13 go go  4096 Mar 21 16:15 tools
17:45:08.691 [go] Current job status: passed.

17:45:08.691 [go] Start to execute task: <exec command="docker" >
<arg>pull</arg>
<arg>gmacario/build-capi-native</arg>
</exec>.
17:45:08.770 Using default tag: latest
17:45:10.879 latest: Pulling from gmacario/build-capi-native
17:45:10.879 6d3a6d998241: Already exists
17:45:10.880 606b08bdd0f3: Already exists
17:45:10.881 1d99b95ffc1c: Already exists
17:45:10.881 a3ed95caeb02: Already exists
17:45:10.882 64cb32515d1f: Already exists
17:45:10.882 27f758fbdce5: Already exists
17:45:10.882 cb86b15f160f: Already exists
17:45:10.883 7e6e2d8a0b33: Already exists
17:45:10.883 fde8067b0a52: Already exists
17:45:10.883 Digest: sha256:00dbd41b31d51f88be393a722009ac16ee21e71de401907128abc80ba1a83997
17:45:10.884 Status: Image is up to date for gmacario/build-capi-native:latest
17:45:10.886 [go] Current job status: passed.

17:45:10.886 [go] Start to execute task: <exec command="docker" >
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
17:45:11.177 Cloning into 'c-poc'...
17:45:16.193 libtoolize: putting auxiliary files in '.'.
17:45:16.194 libtoolize: copying file './ltmain.sh'
17:45:16.452 libtoolize: Consider adding 'AC_CONFIG_MACRO_DIRS([m4])' to configure.ac,
17:45:16.452 libtoolize: and rerunning libtoolize and aclocal.
17:45:16.453 libtoolize: Consider adding '-I m4' to ACLOCAL_AMFLAGS in Makefile.am.
17:45:19.292 configure.ac:18: installing './ar-lib'
17:45:19.297 configure.ac:17: installing './compile'
17:45:19.301 configure.ac:21: installing './config.guess'
17:45:19.305 configure.ac:21: installing './config.sub'
17:45:19.309 configure.ac:16: installing './install-sh'
17:45:19.314 configure.ac:16: installing './missing'
17:45:19.346 Makefile.am: installing './depcomp'
17:45:19.825 checking for a BSD-compatible install... /usr/bin/install -c
17:45:19.832 checking whether build environment is sane... yes
17:45:19.852 checking for a thread-safe mkdir -p... /bin/mkdir -p
17:45:19.853 checking for gawk... no
17:45:19.853 checking for mawk... mawk
17:45:19.879 checking whether make sets $(MAKE)... yes
17:45:19.920 checking whether make supports nested variables... yes
17:45:19.935 checking for gcc... gcc
17:45:20.054 checking whether the C compiler works... yes
17:45:20.055 checking for C compiler default output file name... a.out
17:45:20.125 checking for suffix of executables...
17:45:20.209 checking whether we are cross compiling... no
17:45:20.255 checking for suffix of object files... o
17:45:20.298 checking whether we are using the GNU C compiler... yes
17:45:20.341 checking whether gcc accepts -g... yes
17:45:20.413 checking for gcc option to accept ISO C89... none needed
17:45:20.489 checking whether gcc understands -c and -o together... yes
17:45:20.499 checking for style of include used by make... GNU
17:45:20.607 checking dependency style of gcc... gcc3
17:45:20.609 checking for ar... ar
17:45:20.651 checking the archiver (ar) interface... ar
17:45:20.770 checking build system type... x86_64-pc-linux-gnu
17:45:20.771 checking host system type... x86_64-pc-linux-gnu
17:45:20.777 checking how to print strings... printf
17:45:20.786 checking for a sed that does not truncate output... /bin/sed
17:45:20.793 checking for grep that handles long lines and -e... /bin/grep
17:45:20.797 checking for egrep... /bin/grep -E
17:45:20.801 checking for fgrep... /bin/grep -F
17:45:20.811 checking for ld used by gcc... /usr/bin/ld
17:45:20.817 checking if the linker (/usr/bin/ld) is GNU ld... yes
17:45:20.822 checking for BSD- or MS-compatible name lister (nm)... /usr/bin/nm -B
17:45:20.871 checking the name lister (/usr/bin/nm -B) interface... BSD nm
17:45:20.871 checking whether ln -s works... yes
17:45:20.905 checking the maximum length of command line arguments... 1572864
17:45:20.922 checking how to convert x86_64-pc-linux-gnu file names to x86_64-pc-linux-gnu format... func_convert_file_noop
17:45:20.923 checking how to convert x86_64-pc-linux-gnu file names to toolchain format... func_convert_file_noop
17:45:20.923 checking for /usr/bin/ld option to reload object files... -r
17:45:20.925 checking for objdump... objdump
17:45:20.927 checking how to recognize dependent libraries... pass_all
17:45:20.929 checking for dlltool... no
17:45:20.930 checking how to associate runtime and link libraries... printf %s\n
17:45:20.991 checking for archiver @FILE support... @
17:45:20.993 checking for strip... strip
17:45:20.994 checking for ranlib... ranlib
17:45:21.175 checking command to parse /usr/bin/nm -B output from gcc object... ok
17:45:21.181 checking for sysroot... no
17:45:21.251 checking for a working dd... /bin/dd
17:45:21.260 checking how to truncate binary pipes... /bin/dd bs=4096 count=1
17:45:21.297 checking for mt... no
17:45:21.304 checking if : is a manifest tool... no
17:45:21.358 checking how to run the C preprocessor... gcc -E
17:45:21.627 checking for ANSI C header files... yes
17:45:21.693 checking for sys/types.h... yes
17:45:21.769 checking for sys/stat.h... yes
17:45:21.851 checking for stdlib.h... yes
17:45:21.934 checking for string.h... yes
17:45:22.018 checking for memory.h... yes
17:45:22.102 checking for strings.h... yes
17:45:22.187 checking for inttypes.h... yes
17:45:22.274 checking for stdint.h... yes
17:45:22.365 checking for unistd.h... yes
17:45:22.453 checking for dlfcn.h... yes
17:45:22.465 checking for objdir... .libs
17:45:22.678 checking if gcc supports -fno-rtti -fno-exceptions... no
17:45:22.679 checking for gcc option to produce PIC... -fPIC -DPIC
17:45:22.726 checking if gcc PIC flag -fPIC -DPIC works... yes
17:45:22.867 checking if gcc static flag -static works... yes
17:45:22.939 checking if gcc supports -c -o file.o... yes
17:45:22.940 checking if gcc supports -c -o file.o... (cached) yes
17:45:22.958 checking whether the gcc linker (/usr/bin/ld -m elf_x86_64) supports shared libraries... yes
17:45:23.030 checking whether -lc should be explicitly linked in... no
17:45:23.194 checking dynamic linker characteristics... GNU/Linux ld.so
17:45:23.195 checking how to hardcode library paths into programs... immediate
17:45:23.200 checking whether stripping libraries is possible... yes
17:45:23.201 checking if libtool supports shared libraries... yes
17:45:23.201 checking whether to build shared libraries... yes
17:45:23.202 checking whether to build static libraries... no
17:45:23.205 checking for pkg-config... /usr/bin/pkg-config
17:45:23.217 checking pkg-config is at least version 0.9.0... yes
17:45:23.234 checking for LIBSYSTEMD... yes
17:45:23.399 checking for sd_bus_open in -lsystemd... yes
17:45:23.487 checking for sd_bus_get_scope in -lsystemd... yes
17:45:23.576 checking whether SD_EVENT_INITIAL is declared... yes
17:45:23.643 checking that generated files are newer than configure... done
17:45:23.644 configure: creating ./config.status
17:45:24.584 config.status: creating Makefile
17:45:24.609 config.status: creating capic.pc
17:45:24.634 config.status: creating config.h
17:45:24.652 config.status: executing depfiles commands
17:45:24.698 config.status: executing libtool commands
17:45:24.801 make  all-am
17:45:24.808 make[1]: Entering directory '/home/build/c-poc'
17:45:24.830 /bin/bash ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.    -I./src -I.  -Wall -Werror -fvisibility=hidden -g -O2 -MT src/libcapic_la-backend.lo -MD -MP -MF src/.deps/libcapic_la-backend.Tpo -c -o src/libcapic_la-backend.lo `test -f 'src/backend.c' || echo './'`src/backend.c
17:45:24.946 libtool: compile:  gcc -DHAVE_CONFIG_H -I. -I./src -I. -Wall -Werror -fvisibility=hidden -g -O2 -MT src/libcapic_la-backend.lo -MD -MP -MF src/.deps/libcapic_la-backend.Tpo -c src/backend.c  -fPIC -DPIC -o src/.libs/libcapic_la-backend.o
17:45:25.144 mv -f src/.deps/libcapic_la-backend.Tpo src/.deps/libcapic_la-backend.Plo
17:45:25.150 /bin/bash ./libtool  --tag=CC   --mode=link gcc -I./src -I.  -Wall -Werror -fvisibility=hidden -g -O2 -no-undefined -version-info 0:0:0  -o libcapic.la -rpath /usr/local/lib  src/libcapic_la-backend.lo  
17:45:25.319 libtool: link: gcc -shared  -fPIC -DPIC  src/.libs/libcapic_la-backend.o    -g -O2   -Wl,-soname -Wl,libcapic.so.0 -o .libs/libcapic.so.0.0.0
17:45:25.359 libtool: link: (cd ".libs" && rm -f "libcapic.so.0" && ln -s "libcapic.so.0.0.0" "libcapic.so.0")
17:45:25.373 libtool: link: (cd ".libs" && rm -f "libcapic.so" && ln -s "libcapic.so.0.0.0" "libcapic.so")
17:45:25.400 libtool: link: ( cd ".libs" && rm -f "libcapic.la" && ln -s "../libcapic.la" "libcapic.la" )
17:45:25.416 make[1]: Leaving directory '/home/build/c-poc'
17:45:25.541 make[1]: Entering directory '/home/build/c-poc'
17:45:25.549  /bin/mkdir -p '/usr/local/lib'
17:45:25.552  /bin/bash ./libtool   --mode=install /usr/bin/install -c   libcapic.la '/usr/local/lib'
17:45:25.604 libtool: install: /usr/bin/install -c .libs/libcapic.so.0.0.0 /usr/local/lib/libcapic.so.0.0.0
17:45:25.608 libtool: install: (cd /usr/local/lib && { ln -s -f libcapic.so.0.0.0 libcapic.so.0 || { rm -f libcapic.so.0 && ln -s libcapic.so.0.0.0 libcapic.so.0; }; })
17:45:25.612 libtool: install: (cd /usr/local/lib && { ln -s -f libcapic.so.0.0.0 libcapic.so || { rm -f libcapic.so && ln -s libcapic.so.0.0.0 libcapic.so; }; })
17:45:25.616 libtool: install: /usr/bin/install -c .libs/libcapic.lai /usr/local/lib/libcapic.la
17:45:25.699 libtool: finish: PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/sbin" ldconfig -n /usr/local/lib
17:45:25.887 ----------------------------------------------------------------------
17:45:25.888 Libraries have been installed in:
17:45:25.888    /usr/local/lib
17:45:25.888
17:45:25.889 If you ever happen to want to link against installed libraries
17:45:25.889 in a given directory, LIBDIR, you must either use libtool, and
17:45:25.890 specify the full pathname of the library, or use the '-LLIBDIR'
17:45:25.890 flag during linking and do at least one of the following:
17:45:25.891    - add LIBDIR to the 'LD_LIBRARY_PATH' environment variable
17:45:25.891      during execution
17:45:25.891    - add LIBDIR to the 'LD_RUN_PATH' environment variable
17:45:25.892      during linking
17:45:25.892    - use the '-Wl,-rpath -Wl,LIBDIR' linker flag
17:45:25.893    - have your system administrator add LIBDIR to '/etc/ld.so.conf'
17:45:25.893
17:45:25.893 See any operating system documentation about shared libraries for
17:45:25.895 more information, such as the ld(1) and ld.so(8) manual pages.
17:45:25.895 ----------------------------------------------------------------------
17:45:25.895  /bin/mkdir -p '/usr/local/lib/pkgconfig'
17:45:25.903  /usr/bin/install -c -m 644 capic.pc '/usr/local/lib/pkgconfig'
17:45:25.909  /bin/mkdir -p '/usr/local/include/capic'
17:45:25.920  /usr/bin/install -c -m 644 src/capic/backend.h src/capic/log.h src/capic/dbus-private.h '/usr/local/include/capic'
17:45:25.923 make[1]: Leaving directory '/home/build/c-poc'
17:45:26.139 [go] Current job status: passed.

17:45:26.139 [go] Start to execute task: <exec command="sh" >
<arg>-c</arg>
<arg>pwd</arg>
<arg>ls -la</arg>
<arg>ls -la c-poc</arg>
</exec>.
17:45:26.147 /var/lib/go-agent/pipelines/build_capi_native
17:45:26.274 [go] Current job status: passed.

17:45:26.292 [go] Start to create properties build_capi_native/13/defaultStage/1/defaultJob on 0f29bb7cc96f [/var/lib/go-agent]
17:45:26.292 [go] Start to upload build_capi_native/13/defaultStage/1/defaultJob on 0f29bb7cc96f [/var/lib/go-agent]
17:45:26.461 [go] Job completed build_capi_native/13/defaultStage/1/defaultJob on 0f29bb7cc96f [/var/lib/go-agent]
```

<!-- EOF -->
