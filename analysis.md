# Analysis

## Training and evaluation outputs
The primary nonvolatile outputs from the training and testing processes are persisted in two locations: the S3 DeepRacer bucket, and the CloudWatch logfile. The CloudWatch logfile contains traces of the intermediate training state, which makes it particularly helpful in assessing the quality of the action function and convergence of the model.

### S3 file manifest
| Path                                              | File Name                         | Content Summary                                                                 |
| ------------------------------------------------- | --------------------------------- | ------------------------------------------------------------------------------- | 
|/DeepRacer-Metrics	                                | EvaluationMetrics-[1].json        | Summary results of evaluation. Data is presented at "trial" granularity.	      |
|/DeepRacer-Metrics	                                | TrainingMetrics-[1].json          | Summary results of training. Data is presented at "Episode" granularity.	      |
|/DeepRacer-SageMaker-RoboMaker-comm-[SERIAL]/ip    | done                              | Contains the word "done". Appears to be used as a mutex	|
|/DeepRacer-SageMaker-RoboMaker-comm-[SERIAL]/ip    | hyperparameters.json              | Model hyperparameters		
|/DeepRacer-SageMaker-RoboMaker-comm-[SERIAL]/ip    | ip.json	                        | An IP address	    |
|/DeepRacer-SageMaker-RoboMaker-comm-[SERIAL]/model | .coach_checkpoint			        ||
|/DeepRacer-SageMaker-RoboMaker-comm-[SERIAL]/model | .ready			                ||
|/DeepRacer-SageMaker-RoboMaker-comm-[SERIAL]/model | [1]_Step-[2].ckpt.data-[3]-of-[4]	||		
|/DeepRacer-SageMaker-RoboMaker-comm-[SERIAL]/model | [1]_Step-[2].ckpt.index			||
|/DeepRacer-SageMaker-RoboMaker-comm-[SERIAL]/model | [1]_Step-[2].ckpt.meta			||
|/DeepRacer-SageMaker-RoboMaker-comm-[SERIAL]/model | model_[1].pb			            ||
|/DeepRacer-SageMaker-RoboMaker-comm-[SERIAL]/model | model_metadata.json	            | Vehicle configuration and action space|
|/DeepRacer-SageMaker-RoboMaker-comm-[SERIAL]       | eval_params_[1].yaml	            | Contains an index of services and S3 files attached to the evaluation instance|
|/DeepRacer-SageMaker-RoboMaker-comm-[SERIAL]       | training_params_[1].yaml	        | Contians an index of services and S3 files attached to the training instance|
|/DeepRacer-SageMaker-rlmdl-[SERIAL]/dr-sm-rltj--[SERIAL]/output    |	model.tar.gz    | Trained model weights file|
|/model-metadata/[MODEL_NAME]                       | model_metadata.json               | Describes the configuration of the deepracer used to train the model|
|/reward-functions/[MODEL_NAME]                     | reward_function.py	            | The python reward function entered into the deepracer console|

#### EvaluationMetrics-[1].json
| Field                                             | Description                       |
| ------------------------------------------------- | --------------------------------- | 
| metric_time||
| elapsed_time_in_milliseconds||
| start_time||
| completion_percentage||
| trial||
| episode_status||