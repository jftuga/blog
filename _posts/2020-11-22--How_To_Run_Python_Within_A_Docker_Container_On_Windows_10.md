
---
layout: post
title: "How To Run Python Within A Docker Container On Windows 10"
date: 2020-11-22 23:20:32 +0000
categories: python docker windows
---

This is a brief tutorial on how to run Python in a Docker container.  It's not meant to be an all inclusive dissertation on Docker but rather a *let's get you going* style brief. It assumes that you are comfortable using the command-line, *cmd.exe*.

___


**Docker**

* Install [Docker Desktop](https://www.docker.com/products/docker-desktop) for Windows. This will probably require a reboot for installation. 
* You will want to use the `servercore` Docker image.
* * To install: `docker pull mcr.microsoft.com/windows/servercore:ltsc2019`

For my setup, I have a directory named `c:\temp\outside`. This will become my work area once the Docker container is running.  Any files placed underneath this folder with be visible from within the container. Inside the container, this folder will be mapped to `c:\inside`.

To start the container:

`docker run -t -i --rm --mount type=bind,src=c:\temp\outside,dst=c:\inside -w c:\inside mcr.microsoft.com/windows/servercore:ltsc2019`

Option | Explanation
-------|------------
-t     | Allocate a pseudo-TTY. This is needed for an interactive shell.
-i     | Keep STDIN open even if not attached.
--rm   | Automatically remove the container when it exits.  Your files will still safely reside under `c:\temp\outside` once you have exited the container.
--mount | Attach a filesystem mount to the container.  This has three sub parameters: `type`, `src`, and `dst`.
-w     | Working directory inside the container. There is where you will start off at once inside the container.
(container) | The `container` name plus the `version`. The version comes after the `:`.

___

**Workflow**

You can detach from a running container by using the `CTRL-p CTRL-q` key sequence.  This will place you back outside of the container.

You can then *attach* to get back inside the container but will first need to know the `CONTAINER ID`.

To get the `CONTAINER ID`, run this command:

`docker container ls`

Example output:

```
CONTAINER ID        IMAGE                                           COMMAND                    CREATED             STATUS              PORTS               NAMES
9bad4fb6a956        mcr.microsoft.com/windows/servercore:ltsc2019   "c:\\windows\\system32"   2 minutes ago       Up 2 minutes                            pensive_mclaren
```

To *attach*, run this command:

`docker container attach 9bad4fb6a956`

You can also just use `9b` for the `CONTAINER ID`.  This argument just needs to be a unique id vs any other running container IDs.  Notice that once you are back inside the container, your prompt will change back to `c:\inside`.

**TIP:** When running the `Python` interpreter inside the container, you can not press `CTRL-Z` to exit the interpreter.  If you do this, the container will become unresponsive.  When this happens you will need to press `CTRL-p CTRL-q` to detach from the container and then run `docker container attach` to restart.  The cleanest way to exit the interpreter is to issue the `exit()` command.

___

**Python**

Start your container by issuing the previously mentioned `docker run` command.

To install `Python` and `Git` you will first need to install the [nuget package manager](https://docs.microsoft.com/en-us/nuget/reference/nuget-exe-cli-reference):

```
curl -L https://aka.ms/nugetclidl -o nuget.exe
```

You should now have a `c:\inside\nuget.exe` binary.

Even though `Python 3.9` is the current version, let's install an older version just for fun. If you want to install the current repository version then you can exclude `-Version %PYVER%` 

```
rem install an older version of Python
set PYVER=3.7.5
nuget.exe install python -ExcludeVersion -Version %PYVER% -OutputDirectory .

rem install git -- this can take a while to install
nuget.exe install GitForWindows -ExcludeVersion -OutputDirectory .

rem set the PATH environment variable so that Python and git can run
set PATH=%PATH%;c:\inside\python\tools;c:\inside\python\tools\Scripts;c:\inside\GitForWindows\tools\cmd
```

`Pip` is not installed with this method, but you can run `python -m pip` instead. For example:

```
python -m pip install pyinstaller
```

___

**Example**

This will build a stand-alone [speedtest-cli](https://github.com/sivel/speedtest-cli) Windows executable.

```
rem install PyInstaller as previously mentioned
rem start off in the c:\inside directory
git clone https://github.com/sivel/speedtest-cli
cd speedtest-cli
pyinstaller -F --noupx speedtest.py

rem build output excluded

cd dist
copy speedtest.exe c:\inside
dir speedtest.exe | findstr "exe"

11/22/2020  03:09 PM         5,795,380 speedtest.exe

```

You can now exit the Docker container by running `exit`. Notice that the command prompt has changed your path location.

Run your newly created executable:

```
cd c:\temp\outside
speedtest.exe --version

speedtest-cli 2.1.2
Python 3.7.5 (tags/v3.7.5:5c02a39a0b, Oct 15 2019, 00:11:34) [MSC v.1916 64 bit (AMD64)]
```

___

**Clean Up**

This is a 3 step process:

**Step 1**

Stop and remove any running containers.  Note that there may not be any running containers.

```
rem To get any containers still running
docker container ls

docker container kill CONTAINER-ID

docker container rm CONTAINER-ID
```

**Step 2 - Optional**

Remove the docker image from your system.

```

rem get a list of installed images
docker image ls

docker image rm mcr.microsoft.com/windows/servercore:ltsc2019
```

**Step 3 - Optional**

Remove the installed Python and git folders.

```
rd /s/q c:\temp\outside\python
rd /s/q c:\temp\outside\GitForWindows
rd /s/q c:\temp\outside\speedtest-cli
del c:\temp\outside\nuget.exe
```

This only file that should now be remaining is the `speedtest.exe` file that you just created.

___

Comments, corrections, and suggestions are appreciated. 

Thanks.

