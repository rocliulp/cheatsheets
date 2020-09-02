# environment variable
To pass specific environment variable to specific process only without define it in parent session
``` bash
env TZ=Asia/Shanghai date +"%Y-%m-%d %H:%M:%S %z"
scp -P 1111 locfile user@127.0.0.1:/remotepath/
```