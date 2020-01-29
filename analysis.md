# Analysis


## Training and evaluation outputs
A substantial amount of data about model performance is available via channels beyond the Deepracer Console summary statistics and Kinesis feed. The nonvolatile outputs from the training and testing processes are persisted in two locations: the S3 DeepRacer bucket, and the CloudWatch logfile. The CloudWatch logfile contains traces of the intermediate training state, which makes it particularly helpful in assessing the quality of the reward function and convergence of the model.

The S3 bucket contains file dumps with final high level metrics gathered from the training and evaluation, the robomaker environment settings, the trained model weight files, and the python reward function.

### S3 file manifest

| Path                                              | File Name                         | Content Summary                                                                 |
| ------------------------------------------------- | --------------------------------- | ------------------------------------------------------------------------------- | 
|/DeepRacer-Metrics	                                | EvaluationMetrics-[1].json        | Summary results of evaluation. Data is presented at "trial" granularity.	      |
|/DeepRacer-Metrics	                                | TrainingMetrics-[1].json          | Summary results of training. Data is presented at "Episode" granularity.	      |
|/DeepRacer-SageMaker-RoboMaker-comm-[SERIAL]/ip    | done                              | Contains the word "done". Appears to be used as a semaphore |
|/DeepRacer-SageMaker-RoboMaker-comm-[SERIAL]/ip    | hyperparameters.json              | Model hyperparameters		
|/DeepRacer-SageMaker-RoboMaker-comm-[SERIAL]/ip    | ip.json	                        | An IP address	|
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

#### TrainingMetrics-[1].json
| Field                                             | Description                       |
| ------------------------------------------------- | --------------------------------- | 
| completion_percentage||
| episode||
| start_time||
| metric_time||
| episode_status||
| reward_score||
| elapsed_time_in_milliseconds||

#### hyperparameters.json 
| Field                                             | Description                       |
| ------------------------------------------------- | --------------------------------- | 
| batch_size||
| beta_entropy||
| discount_factor||
| e_greedy_value||
| epsilon_steps||
| exploration_type||
| loss_type||
| lr||
| num_episodes_between_training||
| num_epochs||
| stack_size||
| term_cond_avg_score||
| term_cond_max_episodes||

#### eval_params_[1].yaml
| Field                                             | Description                       |
| ------------------------------------------------- | --------------------------------- | 
| METRICS_S3_OBJECT_KEY||
| AWS_REGION||
| NUMBER_OF_TRIALS||
| CAR_NAME||
| KINESIS_VIDEO_STREAM_NAME||
| WORLD_NAME||
| ROBOMAKER_SIMULATION_JOB_ACCOUNT_ID||
| METRICS_S3_BUCKET||
| MODEL_S3_PREFIX||
| RACE_TYPE||
| JOB_TYPE||
| MODEL_S3_BUCKET||
| CAR_COLOR||


#### training_params_[1].yaml
| Field                                             | Description                       |
| ------------------------------------------------- | --------------------------------- | 
| SAGEMAKER_SHARED_S3_PREFIX||
| CHANGE_START_POSITION||
| METRICS_S3_OBJECT_KEY||
| AWS_REGION||
| NUMBER_OF_EPISODES||
| KINESIS_VIDEO_STREAM_NAME||
| TRAINING_JOB_ARN||
| REWARD_FILE_S3_KEY||
| METRIC_NAMESPACE||
| ALTERNATE_DRIVING_DIRECTION||
| WORLD_NAME||
| ROBOMAKER_SIMULATION_JOB_ACCOUNT_ID||
| METRICS_S3_BUCKET||
| SAGEMAKER_SHARED_S3_BUCKET||
| RACE_TYPE||
| JOB_TYPE||
| MODEL_METADATA_FILE_S3_KEY||
| METRIC_NAME||
| TARGET_REWARD_SCORE||

#### model_metadata.json
| Field                                             | Description                       |
| ------------------------------------------------- | --------------------------------- | 
| action_space||
| sensor||
| neural_network||
| version||
