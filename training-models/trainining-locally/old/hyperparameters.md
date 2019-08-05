To change the default hyper-parameters uncomment the fields that you wish to override and set their value in [rl_deepracer_coach_robomaker.py](https://github.com/crr0004/deepracer/blob/master/rl_coach/rl_deepracer_coach_robomaker.py#L109), in the `hyperparameters` variable.

The default values come from [sagemaker_graph_manager.py](https://github.com/crr0004/deepracer/blob/master/rl_coach/src/markov/sagemaker_graph_manager.py)

|Hyper-parameter name|Default Value|
|---|---|
|batch_size|64|
|num_epochs|10|
|stack_size|1|
|lr|0.0003|
|exploration_type|categorical|
|e_greedy_value|0.05|
|epsilon_steps|10000|
|beta_entropy|0.01|
|discount_factor|0.999|
|loss_type|Mean squared error|
|num_episodes_between_training|20|
|term_cond_max_episodes|100000|
|term_cond_avg_score|100000|
