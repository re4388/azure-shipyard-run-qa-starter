# QA Inference executor (2020-07-06 updated)
## Information
- **Format**: Docker Image 
- **Image Name**: qacompute
- **Image Tag**: 0.1.01
- **Size**: ~20GB
- **Requirements**
  - Docker
  - Nvidia-docker
  - Nvidia-430 (driver), cuda-10.2
  
## Usage:
```sh
$ docker run --rm -d \
-e ENV_RES_STORAGE=<ENV_RES_STORAGE> \
-e ENV_QASERVICE_API_URL=<ENV_QASERVICE_API_URL> \
-e ENV_LOG_PATH=<ENV_LOG_PATH> \
-e PYTHONUNBUFFERED=1 \
--runtime='nvidia' \
qacompute:<tag> \
python main.py --job-id="<job_id>" \
--keyword="<keyword>" \
--question="<question>" \
--num-papers=<num_papers> \
--date-start="<date_start>" \
--date-end="<date_end>"
<mode>
```

## Parameters:
- **ENV_RES_STORAGE**: local storage path
- **ENV_QASERVICE_API_URL**: callback api (not implemented)
- **ENV_LOG_PATH**: path to save log files (not implemented)
- **mode**: paper searching mode. Include candidates:
  - quantit': constraint searching by set the maximum number of searched paper (by keyword)
  - date: constraint searching by date
- **job_id**: an identity string or number for this job 
- **keyword**: keyword string to find papers
- **question**: question to ask these papers
- **num_papers**: an integer. Maximum number of searched paper. Use only in quantity mode.
-	**date_start**: YYYY/MM/DD. Use only in "quantity" mode. Example: 2020/01/01
-	**date_end**: YYYY/MM/DD. Use only in "date" mode. Example: 2020/01/01

## Examples:
### Example 1: quantity mode
```sh
$ docker run --rm -d \
-e ENV_RES_STORAGE="/qadata/" \
-e PYTHONUNBUFFERED=1 \
--runtime="nvidia" \
qacompute:0.1.01 \
python main.py --job-id=1234 --keyword="covid19" --question="what is covid19???" --num-papers=100 quantity
```
### Example 2: date mode
```sh
$ docker run --rm -d \
-e ENV_RES_STORAGE="/qadata/" \
-e PYTHONUNBUFFERED=1 \
--runtime="nvidia" \
qacompute:0.1.01 \
python main.py --job-id=1234 --keyword="covid19" --question="what is covid19???" –date-start="2020/04/01" –date-end="2020/06/01" date
```
### Example 3: use azure shipyard
```sh
...
-e ENV_RES_STORAGE="$AZ_BATCH_TASK_WORKING_DIR" \
...
```
### Example 4: use CPU
```sh
$ docker run --rm -d \
-e ENV_RES_STORAGE="/qadata/" \
-e PYTHONUNBUFFERED=1 \
qacompute:0.1.01 \
python main.py --job-id=1234 --keyword="covid19" --question="what is covid19???" --num-papers=100 quantity
```
