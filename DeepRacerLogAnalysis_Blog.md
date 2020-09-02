# How I used Log Analysis to Drive Experiments & Win the AWS DeepRacer F1 ProAm Race
*A Data-driven Approach to Training & Tuning AWS DeepRacer Reinforcement Learning Models*

## What is AWS DeepRacer?
AWS DeepRacer is the world’s first global autonomous racing league, driven by Reinforcement Learning. It is also a fun and easy way for everyone to get started with Machine Learning, regardless of their background. As part of our digital transformation journey at DBS Bank, we are using AWS DeepRacer as a learning platform to equip more than 3000 of our employees with skills in AI/ML by the end of 2020. Thanks to DeepRacer's virtual simulation and training environment, our employees are able to upgrade their skills and pick up new knowledge, even when they are not physically in the office. The ability to run private races also allows us to create our own racing league, where our employees can pit their newfound skills against each other competitively.

## Winning the F1 ProAm Race in May 2020
In May 2020, racers from around the world were given a unique opportunity to test their Machine Learning skills against F1 professionals in the AWS DeepRacer League F1 ProAm Event. We got to train our models on a replica of the F1 Spanish Grand Prix track, and the top 10 racers from the month-long Head-to-Head qualifying race would then face off against F1 professional drivers Daniel Ricciardo and Tatiana Calderon in a Grand Prix-style race.

![F1 ProAm Race](/images/log_analysis_blog_f1proamrace.png)

After a gruelling month of racing, I emerged as the Champion in the F1 ProAm Race, beating fellow racer JJ and the pro F1 drivers to the checkered flag! I attribute my win to having performed more experiments than my fellow competitors throughout that month of racing. Those experiments allowed me to continuously tweak and improve my model leading up to the final race at the end of the month. Behind those experiments, are ideas that arose from data-driven insights through Log Analysis.

## What is Log Analysis?
Log Analysis allows us to use a Jupyter Notebook to analyse and debug our models using log data generated from the AWS DeepRacer simulation and training environment. With short snippets of Python code, we can plot and visualise our model's training performance, through various graphs and heatmaps. I created various unique visualisations in my Log Analysis Notebook, and that ultimately helped me to create a model that was fast and stable enough to win the F1 ProAm Race.

![Log Analysis for the Spain F1 Track](/images/log_analysis_blog_visualisations.png)

> In this blog, I will share about some of those visualisations that I created. I will also show how we can make use of Amazon SageMaker to spin up a Notebook Instance to perform Log Analysis for our model training data.

## Amazon SageMaker Notebook Instances
An Amazon SageMaker Notebook Instance is an ML compute instance running the Jupyter Notebook application. Amazon SageMaker manages creating the instance and its related resources, so we can focus on analysing our models training data without worrying about provisioning EC2 resources directly.

## Using Amazon SageMaker Notebook Instance for Log Analysis
One of the biggest benefits of using an Amazon SageMaker Notebook Instance to perform DeepRacer Log Analysis, is that Amazon SageMaker automatically installs Anaconda packages and libraries for common deep learning platforms on our behalf, including TensorFlow deep learning libraries. It also automatically attaches an EBS storage volume to our Notebook instance, which we can use as a persistent working area to perform Log Analysis and store our analysis artifacts.

#### Creating a Notebook Instance
To get started, I simply created a Notebook Instance from the Amazon SageMaker console. From the menu on the left, `Notebook > Notebook instances > Create notebook instance`.

<Pic showing SageMaker console -> Notebook -> Notebook instances -> Create notebook instance>

I only need to fill in my instance name, and ensure that the correct instance type is selected. For DeepRacer Log Analysis, the smallest instance type (`ml.t2.medium`) is usually sufficient for my needs. I will also use the default storage volume size of `5 GB`.

<Pic showing Create notebook instance screen>

Once I have my Notebook Instance started up, I can fire up JupyterLab, the IDE for Jupyter Notebooks.

<Pic showing JupyterLab button and Empty Notebook>

#### Cloning the Log Analysis Repo from JupyterLab
From the JupyterLab IDE, it is extremely easy to clone a Git repository to make use of Log Analysis Notebooks shared by the community. For example, with Git integration, I can clone my [Log Analysis repository](https://github.com/TheRayG/deepracer-log-analysis) in seconds.

![Cloning a Notebook Repo](/images/log_analysis_blog_cloninganotebookrepo.png)

#### Downloading Logs from the AWS DeepRacer Console
To prepare the data that we want to analyse, we will have to download our model training logs from the AWS DeepRacer Console. From the model page of the model that we intend to analyse, navigate to `Training > Resources > Download Logs` to download the training log files, packaged in the form of a .tar.gz file.

<Pic showing model download button>

#### Extracting the Required Log Files for Analysis
Extract the RoboMaker and SageMaker log files from the .tar.gz package (found in `logs/training/`), and then drag / upload the 2 log files into the `logs` folder in the local cloned directory.

<Pic showing folder structure of the .tar.gz>
<Pic showing JupyterLab folder of the local cloned repo>

We're now ready to open up our Log Analysis Notebook to work its magic! Simply navigate into our local cloned directory, and double-click on the .ipynb file to open the Notebook up.

<Pic of launching the Notebook>

Once we have our Notebook opened, the first thing we'll need to do is to specify the filenames of the 2 log files that we had just uploaded into the `logs` folder of our local cloned directory.

<Pic of the 2 files in local cloned directory>

There are numerous markdown descriptions and comments in the Notebook to explain what each cell does. I'll highlight some of the visualisations from that Notebook and explain the thought process behind them below.

## Visualising the Performance Envelope of the Model
One common question asked by beginners of AWS DeepRacer is, "*If 2 models are trained using the same Reward Function and Hyperparameters, why do they have different lap times when I evaluate them?*" I find that this visualisation is a great way to explain that:

<Pic of histogram/normal distribution>

By plotting a histogram of lap times achieved by the model during training, we can work out statistically the average and best-case lap times we can expect from the model. I've noticed that the lap times of the model during training resembles a normal distribution - so I use the -2 and -3 Std Dev markers to show the potential best-case lap times for the model, with 2.275% and 0.135% chance of occurring respectively. This helps me to gauge if I should continue cloning and tweaking the model, or abandon it and start afresh with a different approach.


## Identifying Potential Model Checkpoints for Race Submission

## Identifying Convergence and Gauging Consistency

## Identifying Inefficiencies in Driving Behaviour

## Identifying Track Sections to Tweak Actions & Rewards

## Cleaning Up
