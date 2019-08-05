# Launch Container

To launch an evaluation cycle, you launch the robomaker docker container with `docker run --rm --name dr_e --env-file ./robomaker.env --network sagemaker-local -p 8080:5900 -it crr0004/deepracer_robomaker:console -e "METRICS_S3_OBJECT_KEY=custom_files/eval_metrics.json" -e "NUMBER_OF_TRIALS=5" "./run.sh build evaluation.launch"`

The reason we use `-e "METRICS_S3_OBJECT_KEY=custom_files/eval_metrics.json"` is so the evaluation metrics will be written to an alternate file than the training metric file.

# Change world in evaluation

You can change the world by passing `-e "WORLD_NAME=<track_name>"` into the container command. E.G `docker run --rm --name dr_e --env-file ./robomaker.env --network sagemaker-local -p 8080:5900 -it -e "WORLD_NAME=Tokyo_Training_track" -e "METRICS_S3_OBJECT_KEY=custom_files/eval_metric.json" -e "NUMBER_OF_TRIALS=5" crr0004/deepracer_robomaker:console "./run.sh build evaluation.launch"`.

# Evaluation Metrics

Look in `bucket/custom_files/eval_metrics.json`.
