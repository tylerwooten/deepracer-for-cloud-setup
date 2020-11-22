## Create logs for [jupyter log analysis](EC2-jupyter-log-analysis.md)
* In EC2 root directory, create a bash script with `touch create_log.sh`
* Edit the file (I use `nano create_log.sh`) and add paste the following:
```bash
#!/bin/bash

for name in `docker ps --format "{{.Names}}"`; do
    docker logs ${name} >& ./data/logs/${name}.log
done
```
* Run `chmod +x create_log.sh` to make the file executeable
* Run  `\rm data/logs/* ; ./create_log.sh` when your model is training to generate the logs
