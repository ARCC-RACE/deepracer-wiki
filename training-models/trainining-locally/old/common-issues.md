## FAQ and Issues
## 1
_Got exception while downloading checkpoint An error occurred (404) when calling the HeadObject operation: Not Found when starting sagemaker_

This is might be caused by the sagemaker container looking for a pretrained model, you need to comment out the two hyperparameters inside rl_deepracer_coach_robomaker.py to stop it looking for a pretrained model. See Issue #2 for more info

## 2
_Running sagemaker container gives the error `no module named 'rospy'`_

You will need to copy the config.yaml file to ~/.sagemaker to configure where the temp directories for the sagemaker docker containers are put. 

You either need edit `~/.sagemaker/config.yaml` to point to a directory that exists, or create `../../robo/container`. It is relative to where you run rl_deepracer_coach_robomaker.py from. The defautl is set to a folder a couple directories up.

## 3
_Running as `sudo python` causes errors._

You can try using `sudo -E` to pass environment variables through to the command.

## 4
_Robomaker seems stuck_

There have been manually entered IPs in the robomaker.env file for the crr0004/deepracer_robomaker container. I set the IP it to `http://172.18.0.1:9000` but if you start minio before the containers are started for the first time, that IP won't connect properly.
