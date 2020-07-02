
# Local test Dockerize QA in Azure-batch

## What this app can do
- Run Azure batch with dockerize QA app and send output stram to Azure storage

## Prerequisite
- Python environment. I use conda to setup the environment, you can use whatever virtual env to setup the env.
- This is because shipyard use python, so we need to use python env and install all packaged via `pip install -r requirement.txt`
- you will need to fill-up your own `credentials.yaml`
- you will need to modify the `.yaml` file based on actual situation, please refer to [shipyard doc](https://batch-shipyard.readthedocs.io/en/latest/).
- you will need to setup your own azure serivce, azure storage.
- You will need to have the QA docker image and push to ACR(Azure Container Registry).


## What this app can NOT do (and I am not yet try out)
- hard-code the docker run input => so we will need to get data from Azure db service (this seems possible given [shipyard doc](https://batch-shipyard.readthedocs.io/en/latest/))
- This is local test run. For production, we may need to put the whole service/env in Azure env, e.g. one way of doeing may refer to [this](https://batch-shipyard.readthedocs.io/en/latest/60-batch-shipyard-site-extension/).


## What if I don't want to use shipyard-config way?
- I found this [website](https://www.muspells.net/blog/2018/11/azure-batch-task-in-containers/) talking on batch-run docker quite useful.
- On Docker command, please talk to us on more about QA docker command.


## Shipyard website
- https://batch-shipyard.readthedocs.io/en/latest/


## How to run (after you set up above things..)
- Active the virtual env
-
    `conda activate shipyard-monorepo`

- cd to qa folder

    `cd qa`

- add pool
-
    `..\shipyard pool add`

- add jobs
-
    `..\shipyard jobs add --tail stdout.txt`


after all tasks are done, here is how we clean up.
- delete jobs
-
    `..\shipyard jobs del -y --wait`
- delete pool
-
`..\shipyard pool del -y`


## Personal Note
- Use `--tail` is to live-stram the stdout.txt to console
- Use `PYTHONUNBUFFERED: 1` is to not buffer puthon log so we can see stream output when docker running
- After job run you can use `data` command to get stream data for specific job, task and file
    `..\shipyard data files stream --filespec job6,task-00000,stdout.txt`
- We can use `--disk` to stream data into disk
    `..\shipyard data files stream --disk --filespec job6,task-00000,stdout.txt`
- We only use simple Azure VM size to play around, need to adjust vm-size based on business requirement.
- I found out the time to init the pool take quite a while. Given Business requirement, this might okay. O.W. there're other VM options to choose to speedup the VM kick off processes.

