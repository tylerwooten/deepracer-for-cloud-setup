# run.env

DR_WORLD_NAME =
* **reInvent2019_track**  (the original and best)
* **jyllandsringen_open**  (the "Open" June 2021 virtual race)
* **jyllandsringen_pro**  (the "Pro" June 2021 virtual race)


DR_RACE_TYPE =
* **TIME_TRIAL**
* **OBJECT_AVOIDANCE**
* **HEAD_TO_BOT**

# system.env

DR_ROBOMAKER_IMAGE =
* **4.0.5-gpu**       (latest build number comes from: https://github.com/aws-deepracer-community/deepracer-simapp/releases)

# model_metadata.json

sensor =  _one or more of..._
* **FRONT_FACING_CAMERA**
* **STEREO_CAMERAS**
* **SECTOR_LIDAR**

action_space_type =
* **discrete**
* **continuous**

version =
* **3**  
* **4** (you must use version 4 for best results competing in AWS virtual races)

action_space =    (use this format for continuous, or refer to default for discrete format)
* {"steering_angle": {"high": 30, "low": -30}, "speed": {"high": 4, "low": 1}}


# Master Reference

https://aws-deepracer-community.github.io/deepracer-for-cloud/reference.html
