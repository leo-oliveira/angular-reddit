# Troubeshoot - Local dev with docker containers

- [Troubeshoot - Local dev with docker containers](#troubeshoot---local-dev-with-docker-containers)
  - [Docker falling to start Linux containers](#docker-falling-to-start-linux-containers)
  - [Docker falling to switch between Windows and Linux containers](#docker-falling-to-switch-between-windows-and-linux-containers)
  - [Docker process reserving too much memory and never releasing it](#docker-process-reserving-too-much-memory-and-never-releasing-it)
  - [Docker login to ECR failed with message: Error saving credentials](#docker-login-to-ecr-failed-with-message-error-saving-credentials)
  - [Docker Container Can't Access local database](#docker-container-cant-access-local-database)
  - [Clock De-sync from host to docker containers](#clock-de-sync-from-host-to-docker-containers)
  - [AWS Authentication Configuration Faield](#aws-authentication-configuration-faield)

## Docker falling to start Linux containers
- Windows Version - 2004 Build 19041.508
- Docker Version - 19.03.13, build 4484c46d9d

![](/docs/images/troubleshoot/image01-01.png)

![](/docs/images/troubleshoot/image01-02.png)

As of 2020-10-12 the only fix for this is to reinstall Docker-Desktop

After reinstallation the command wsl -l -v should return (ignore the Ubuntu distro):

![](/docs/images/troubleshoot/image01-03.png)

## Docker falling to switch between Windows and Linux containers
- Windows Version - 2004 Build 19041.508
- Docker Version - 19.03.13, build 4484c46d9d

![](/docs/images/troubleshoot/image02-01.png)

```
Running Windows Containers
Switching Docker daemon to use 'windows' containers...
Docker daemon switched!
Traceback (most recent call last):
File "site-packages\docker\api\client.py", line 205, in _retrieve_server_version
File "site-packages\docker\api\daemon.py", line 181, in version
File "site-packages\docker\utils\decorators.py", line 46, in inner
File "site-packages\docker\api\client.py", line 228, in _get
File "site-packages\requests\sessions.py", line 543, in get
File "site-packages\requests\sessions.py", line 530, in request
File "site-packages\requests\sessions.py", line 643, in send
File "site-packages\requests\adapters.py", line 449, in send
File "site-packages\urllib3\connectionpool.py", line 677, in urlopen
File "site-packages\urllib3\connectionpool.py", line 392, in _make_request
File "http\client.py", line 1244, in request
File "http\client.py", line 1290, in _send_request
File "http\client.py", line 1239, in endheaders
File "http\client.py", line 1026, in _send_output
File "http\client.py", line 966, in send
File "site-packages\docker\transport\npipeconn.py", line 32, in connect
File "site-packages\docker\transport\npipesocket.py", line 23, in wrapped
File "site-packages\docker\transport\npipesocket.py", line 72, in connect
File "site-packages\docker\transport\npipesocket.py", line 59, in connect
pywintypes.error: (2, 'CreateFile', 'The system cannot find the file specified.')
 
During handling of the above exception, another exception occurred:
 
Traceback (most recent call last):
File "docker-compose", line 3, in <module>
File "compose\cli\main.py", line 67, in main
File "compose\cli\main.py", line 123, in perform_command
File "compose\cli\command.py", line 69, in project_from_options
File "compose\cli\command.py", line 132, in get_project
File "compose\cli\docker_client.py", line 43, in get_client
File "compose\cli\docker_client.py", line 170, in docker_client
File "site-packages\docker\api\client.py", line 188, in __init__
File "site-packages\docker\api\client.py", line 213, in _retrieve_server_version
docker.errors.DockerException: Error while fetching server API version: (2, 'CreateFile', 'The system cannot find the file specified.')
[32692] Failed to execute script docker-compose
Traceback (most recent call last):
File "site-packages\docker\api\client.py", line 205, in _retrieve_server_version
File "site-packages\docker\api\daemon.py", line 181, in version
File "site-packages\docker\utils\decorators.py", line 46, in inner
File "site-packages\docker\api\client.py", line 228, in _get
File "site-packages\requests\sessions.py", line 543, in get
File "site-packages\requests\sessions.py", line 530, in request
File "site-packages\requests\sessions.py", line 643, in send
File "site-packages\requests\adapters.py", line 449, in send
File "site-packages\urllib3\connectionpool.py", line 677, in urlopen
File "site-packages\urllib3\connectionpool.py", line 392, in _make_request
File "http\client.py", line 1244, in request
File "http\client.py", line 1290, in _send_request
File "http\client.py", line 1239, in endheaders
File "http\client.py", line 1026, in _send_output
File "http\client.py", line 966, in send
File "site-packages\docker\transport\npipeconn.py", line 32, in connect
File "site-packages\docker\transport\npipesocket.py", line 23, in wrapped
File "site-packages\docker\transport\npipesocket.py", line 72, in connect
File "site-packages\docker\transport\npipesocket.py", line 59, in connect
pywintypes.error: (2, 'CreateFile', 'The system cannot find the file specified.')
 
During handling of the above exception, another exception occurred:
 
Traceback (most recent call last):
File "docker-compose", line 3, in <module>
File "compose\cli\main.py", line 67, in main
File "compose\cli\main.py", line 123, in perform_command
File "compose\cli\command.py", line 69, in project_from_options
File "compose\cli\command.py", line 132, in get_project
File "compose\cli\docker_client.py", line 43, in get_client
File "compose\cli\docker_client.py", line 170, in docker_client
File "site-packages\docker\api\client.py", line 188, in __init__
File "site-packages\docker\api\client.py", line 213, in _retrieve_server_version
docker.errors.DockerException: Error while fetching server API version: (2, 'CreateFile', 'The system cannot find the file specified.')
[6348] Failed to execute script docker-compose
Running Linux Containers
Switching Docker daemon to use 'linux' containers...
Docker daemon switched!
Traceback (most recent call last):
File "site-packages\docker\api\client.py", line 205, in _retrieve_server_version
File "site-packages\docker\api\daemon.py", line 181, in version
File "site-packages\docker\utils\decorators.py", line 46, in inner
File "site-packages\docker\api\client.py", line 228, in _get
File "site-packages\requests\sessions.py", line 543, in get
File "site-packages\requests\sessions.py", line 530, in request
File "site-packages\requests\sessions.py", line 643, in send
File "site-packages\requests\adapters.py", line 449, in send
File "site-packages\urllib3\connectionpool.py", line 677, in urlopen
File "site-packages\urllib3\connectionpool.py", line 392, in _make_request
File "http\client.py", line 1244, in request
File "http\client.py", line 1290, in _send_request
File "http\client.py", line 1239, in endheaders
File "http\client.py", line 1026, in _send_output
File "http\client.py", line 966, in send
File "site-packages\docker\transport\npipeconn.py", line 32, in connect
File "site-packages\docker\transport\npipesocket.py", line 23, in wrapped
File "site-packages\docker\transport\npipesocket.py", line 72, in connect
File "site-packages\docker\transport\npipesocket.py", line 59, in connect
pywintypes.error: (2, 'CreateFile', 'The system cannot find the file specified.')
 
During handling of the above exception, another exception occurred:
 
Traceback (most recent call last):
File "docker-compose", line 3, in <module>
File "compose\cli\main.py", line 67, in main
File "compose\cli\main.py", line 123, in perform_command
File "compose\cli\command.py", line 69, in project_from_options
File "compose\cli\command.py", line 132, in get_project
File "compose\cli\docker_client.py", line 43, in get_client
File "compose\cli\docker_client.py", line 170, in docker_client
File "site-packages\docker\api\client.py", line 188, in __init__
File "site-packages\docker\api\client.py", line 213, in _retrieve_server_version
docker.errors.DockerException: Error while fetching server API version: (2, 'CreateFile', 'The system cannot find the file specified.')
[16876] Failed to execute script docker-compose
Traceback (most recent call last):
File "site-packages\docker\api\client.py", line 205, in _retrieve_server_version
File "site-packages\docker\api\daemon.py", line 181, in version
File "site-packages\docker\utils\decorators.py", line 46, in inner
File "site-packages\docker\api\client.py", line 228, in _get
File "site-packages\requests\sessions.py", line 543, in get
File "site-packages\requests\sessions.py", line 530, in request
File "site-packages\requests\sessions.py", line 643, in send
File "site-packages\requests\adapters.py", line 449, in send
File "site-packages\urllib3\connectionpool.py", line 677, in urlopen
File "site-packages\urllib3\connectionpool.py", line 392, in _make_request
File "http\client.py", line 1244, in request
File "http\client.py", line 1290, in _send_request
File "http\client.py", line 1239, in endheaders
File "http\client.py", line 1026, in _send_output
File "http\client.py", line 966, in send
File "site-packages\docker\transport\npipeconn.py", line 32, in connect
File "site-packages\docker\transport\npipesocket.py", line 23, in wrapped
File "site-packages\docker\transport\npipesocket.py", line 72, in connect
File "site-packages\docker\transport\npipesocket.py", line 59, in connect
pywintypes.error: (2, 'CreateFile', 'The system cannot find the file specified.')
 
During handling of the above exception, another exception occurred:
 
Traceback (most recent call last):
File "docker-compose", line 3, in <module>
File "compose\cli\main.py", line 67, in main
File "compose\cli\main.py", line 123, in perform_command
File "compose\cli\command.py", line 69, in project_from_options
File "compose\cli\command.py", line 132, in get_project
File "compose\cli\docker_client.py", line 43, in get_client
File "compose\cli\docker_client.py", line 170, in docker_client
File "site-packages\docker\api\client.py", line 188, in __init__
File "site-packages\docker\api\client.py", line 213, in _retrieve_server_version
docker.errors.DockerException: Error while fetching server API version: (2, 'CreateFile', 'The system cannot find the file specified.')
[21956] Failed to execute script docker-compose
```

As of 2020-10-12 the only fix for this is to restart Docker-Desktop. If it persists, Docker-Desktop must be reinstalled. 

## Docker process reserving too much memory and never releasing it

You can limit WSL2 memory usage by creating a file in C:\Users\yourUserName\.wslconfig

And specifying the max memory, processors and swap for the WSL2 process.

```
[wsl2] 
memory=8GB
processors=4
swap=0
localhostForwarding=true
```
As a last resort you can shutdown all WSL2 distros by running:

```
wsl --shutdown
```
Docker Desktop will ask to restart the WSL service.

## Docker login to ECR failed with message: Error saving credentials

```
Error saving credentials: error storing credentials - err: exit status 1, out: error storing credentials - err: exit status 1, out:The stub received bad data.
```
One quick workaround is to modify `.docker/config.json` file. 

Remove the following line so docker will use file system to store tokens: 

`"credsStore": "wincred"` (it could be "desktop")

The docker config file is located in `%USERPROFILE%/.docker/config.json`

If the error still shows you can reset the config file to
```json

{
"auths": {
"https://index.docker.io/v1/": {}
},
"stackOrchestrator": "swarm"
}
```
Running the `run-containers` script with the switch `-CleanDockerConfig` will reset the docker config file.

## Docker Container Can't Access local database

Check if the SQL Server Configuration Manager has Named Pipes and TCP/IP enabled on the Network Configuration.

If its disabled, enable it and restart the SqlServer service.

![](/docs/images/troubleshoot/image05-01.png)

Check connectivity from the container to the local sql server:

Open a shell into the container:

```
 docker exec -it microservice-brand bash
 ```

Or using docker desktop UI:

![](/docs/images/troubleshoot/image05-02.png)

Install sqlcmd:
```
curl https://packages.microsoft.com/rhel/7.3/prod/mssql-tools-17.6.1.1-1.x86_64.rpm > .\mssql-tools-17.6.1.1-1.x86_64.rpm
yum localinstall mssql-tools-17.6.1.1-1.x86_64.rpm -y
```

Try to connect to the server:
```
sqlcmd -S host.docker.internal -U "dev" -P "dev"
>>select * from master.dbo.spt_monitor; 
>>go
```

## Clock De-sync from host to docker containers
![](/docs/images/troubleshoot/image05-03.png)

This can happen if you set your host PC to sleep / hibernate.

This is a known Docker bug.

You can run `sudo hwclock -s on` a WSL terminal to fix it.

## AWS Authentication Configuration Faield

```
An error occurred (ExpiredTokenException) when calling the GetAuthorizationToken operation: The security token included in the request is expired
```
Run `aws sso login --profile intelliflo-developers`
