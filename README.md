# deepracer-log-analysis
## *Adventures in a data-driven approach to training, evaluating and tuning AWS DeepRacer reinforcement learning models.*

<br>

This Notebook, which is compatible with logs from the AWS DeepRacer Console after Aug 2020, is a redo of Log Analysis solutions provided in the [AWS DeepRacer Workshops repository](https://github.com/aws-samples/aws-deepracer-workshops). The log analysis here parses log data from AWS RoboMaker (SIM_TRACE_LOG data) and Amazon SageMaker (policy training data), and introduces some analyses that are not present in the AWS samples.

With the new AWS DeepRacer console update in Aug 2020, these logs are no longer streamed to CloudWatch Logs during training. Instead, they are downloadable from the model page in the AWS DeepRacer console, after training has terminated.

The code here:
- is compatible with downloaded logs from the new console (after Aug 2020).
- should be backwards compatible with DeepRacer logs previously downloaded from CloudWatch Logs too.
- is maintained such that it can be run directly from a Amazon SageMaker Notebook instance, as long as the relative paths of the RoboMaker and SageMaker log files are specified correctly.
- does not require access to any AWS Services (hence no awscli or boto3 required) when analysing the log data files.

## Test-drive this Notebook!

To take this Notebook on a quick test drive, just do the following:
- Clone this repo
- Download your own model training log (.tar.gz file) from the AWS DeepRacer console
- Extract the RoboMaker and SageMaker log files (found in `logs/training/` in the .tar.gz), into the `logs` folder
- Spin the .ipynb file up on Jupyter Notebook (works in an Amazon SageMaker Notebook instance too)
- Edit the path to your logs in the Notebook
- Run all cells to test the log analysis on your logs!

## Credits

Thanks to:
- The AWS DeepRacer product and data science teams who had shared the original Log Analysis code as part of the AWS DeepRacer Workshops repository.
- AWS DeepRacer Community members who have shared ideas and contributed in one way or another.

## License

This project retains the license of the aws-deepracer-workshops project from which this was based. Our understanding is that it is a license more permissive than the MIT license and allows for removing of the copyright headers.

Unless explicitly sated otherwise, this license applies to all files in this repository.

## Troubleshooting

If you face problems, do reach out to the [AWS DeepRacer Community Slack](https://deepracing.io/), on the #dr-training-log-analysis channel.

## Contact

You can contact **RayG** through the DeepRacer Community Slack @ [deepracing.io](https://deepracing.io/).
