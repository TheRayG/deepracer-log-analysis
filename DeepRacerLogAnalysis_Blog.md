# How I Used Log Analysis to Drive Experiments & Win the AWS DeepRacer F1 ProAm Race
*A Data-driven Approach to Training & Tuning AWS DeepRacer Reinforcement Learning Models*
<br>
<br>

## What is AWS DeepRacer & How is it Changing the Way People Learn About Machine Learning?
AWS DeepRacer is the world’s first global autonomous racing league, driven by Reinforcement Learning. It is a fun and easy way for individuals to get started with Machine Learning, regardless of skill or background. For companies, it is also powerful platform to facilitate the teaching of Machine Learning to employees at the enterprise level.

At DBS Bank where I work, as part of our digital transformation journey, we are taking innovative steps to future-proof our workforce. We've partnered with AWS to bring the AWS DeepRacer League to DBS to train over 3,000 employees in AI and ML by the end of the year. Thanks to DeepRacer's virtual simulation and training environment, our employees are able to upgrade their skills and pick up new knowledge, even when they are not physically in the office. The ability to run private races also allows us to create our own racing league, where our employees can put their newly-learned skills to the test.
<br>
<br>

## Winning the F1 ProAm Race in May 2020
As an individual racer, I've been active in the AWS DeepRacer League since 2019. In May this year, racers from around the world were given a unique opportunity to pit their Machine Learning skills against F1 professionals in the AWS DeepRacer F1 ProAm Race. We got to train our models on a replica of the F1 Spanish Grand Prix track, and the top 10 racers from the month-long Head-to-Head qualifying race would then face off against F1 professional drivers Daniel Ricciardo and Tatiana Calderon in a Grand Prix-style race.
<br>
<br>

![F1 ProAm Race](/images/log_analysis_blog_f1proamrace.png)
<br>
<br>

After a gruelling month of racing, I emerged as the Champion in the F1 ProAm Race, beating fellow racers and the pro F1 drivers to the checkered flag! Looking back now, I attribute my win to having performed more experiments than my fellow competitors throughout that month of racing. Those experiments allowed me to continuously tweak and improve my model leading up to the final race at the end of the month. Behind those experiments are ideas that arose from data-driven insights through Log Analysis.
<br>
<br>

## What is Log Analysis?
Log Analysis allows us to use a Jupyter Notebook to analyse and debug our models based on log data generated from the AWS DeepRacer simulation and training environment. With snippets of Python code, we can plot and visualise our model's training performance through various graphs and heatmaps. I created several unique visualisations that ultimately helped me to train a model that was fast and stable enough to win the F1 ProAm Race.
<br>
<br>

![Log Analysis for the Spain F1 Track](/images/log_analysis_blog_visualisations.png)
<br>
<br>

<h2>

> *In this blog, I will share about some of the visualisations that I created, and show how we can make use of Amazon SageMaker to spin up a Notebook instance to perform Log Analysis on our DeepRacer model training data.*

</h2>
<br>

If you are already familiar with opening Notebooks in a JupyterLab Notebook application, you can **[skip directly](#ready-to-go)** to the Log Analysis stuff.
<br>
<br>

## Amazon SageMaker Notebook Instances
An Amazon SageMaker Notebook instance is a managed ML compute instance running the Jupyter Notebook application. Amazon SageMaker manages creating the instance and its related resources, so we can focus on analysing our model training data without worrying about provisioning EC2 or storage resources directly.
<br>
<br>

## Using Amazon SageMaker Notebook Instance for Log Analysis
One of the greatest benefits of using an Amazon SageMaker Notebook instance to perform DeepRacer Log Analysis, is that Amazon SageMaker automatically installs Anaconda packages and libraries for common deep learning platforms on our behalf, including TensorFlow deep learning libraries. It also automatically attaches an ML storage volume to our Notebook instance, which we can use as a persistent working storage to perform Log Analysis and retain our analysis artifacts.
<br>
<br>

#### Creating a Notebook Instance
To get started, simply create a Notebook instance from the Amazon SageMaker console. From the menu on the left, `Notebook > Notebook instances > Create notebook instance`.
<br>
<br>

![Creating a new SageMaker Notebook Instance](/images/log_analysis_blog_sagemakernotebookinstancepage.png)
<br>
<br>

We'll only need to fill in our Notebook instance name, and ensure that the correct instance type is selected. For DeepRacer Log Analysis, the smallest instance type (`ml.t2.medium`) is usually sufficient. Let's use the default storage volume size of `5 GB` for a start.
<br>
<br>

![Create Notebook Instance Screen](/images/log_analysis_blog_creatingnotebookinstance.png)
<br>
<br>

Once the Notebook instance is started up with an "InService" status, we can fire up JupyterLab, the IDE for Jupyter Notebooks.
<br>
<br>

![Notebook Instance In Service](/images/log_analysis_blog_notebookinstanceinservice.png)
<br>
<br>

#### Cloning the Log Analysis Repo from JupyterLab
From the JupyterLab IDE, we can easily clone a Git repository to make use of Log Analysis Notebooks shared by the community. For example, I can clone [my Log Analysis repository](https://github.com/TheRayG/deepracer-log-analysis) in seconds. To clone from my Log Analysis repository, use `https://github.com/TheRayG/deepracer-log-analysis.git` as the URI.
<br>
<br>

![Cloning a Notebook Repo](/images/log_analysis_blog_cloninganotebookrepo.png)
<br>
<br>

After cloning the repository, we should see it appear in the folder structure on the left side of the JupyterLab IDE.
<br>
<br>

![JupterLab Screen After Cloning a Repo](/images/log_analysis_blog_jupyterlabwithclonedrepo.png)
<br>
<br>

#### Downloading Logs from the AWS DeepRacer Console
To prepare the data that we want to analyse, we will have to download our model training logs from the AWS DeepRacer Console. From the model page of the model that we intend to analyse, navigate to `Training > Resources > Download Logs` to download the training log files, which will be packaged in a .tar.gz file.
<br>
<br>

![Downloading the DeepRacer Logs](/images/log_analysis_blog_downloadingdeepracerlogs.png)
<br>
<br>

#### Extracting the Required Log Files for Analysis
Extract the RoboMaker and SageMaker log files from the .tar.gz package (found in `logs/training/` sub-directory).
<br>
<br>

![Extract the Log Files](/images/log_analysis_blog_deepracerlogsstructure.png)
<br>
<br>

Then drag or upload the 2 log files into the `/deepracer-log-analysis/logs` folder in the JupyterLab IDE.
<br>
<br>

![Uploading the Log Files](/images/log_analysis_blog_uploadedtraininglogs.png)
<br>
<br>

We're now ready to open up our Log Analysis Notebook to work its magic! Simply navigate into the `/deepracer-log-analysis` folder on the left side of the IDE, then double-click on the .ipynb file to open up the Notebook. While opening the Notebook, you may be prompted a kernel. Choose a kernel that uses Python 3, such as `conda_tensorflow_p36`.
<br>
<br>

![Selecting the Kernel](/images/log_analysis_blog_notebookkernelselection.png)
<br>
<br>

Once we have our Notebook opened and the kernel selected, wait till the kernel status on the bottom left of the screen changes from "Starting" to "Idle". After that, edit the Notebook to specify the path and names of the 2 log files that we have just uploaded.
<br>
<br>

![Specifying Our Log Filenames](/images/log_analysis_blog_notebookspecifylogfilenames.png)
<br>
<br>

## Ready to Go!
We can now run the Notebook by clicking on `Run > Run All Cells` in JupyterLab. There are numerous markdown descriptions and comments in my Log Analysis Notebook to explain what each cell does. I'll highlight some of the visualisations from that Notebook and explain some of the thought processes behind them below.
<br>
<br>

## Visualising the Performance Envelope of the Model
Here is a common question asked by beginners of AWS DeepRacer (and I find that this visualisation is a great way to explain it):
<h2>
  
> "*If 2 models are trained for the same amount of time using the same Reward Function and Hyperparameters, why do they have different lap times when I evaluate them?*"

</h2>
<br>
<br>

![Histogram of Lap Times](/images/log_analysis_blog_laptimehistogram.png)
<br>
<br>

I use this to illustrate the "performance envelope" of my model. By plotting a histogram of lap times achieved by the model during training, we can show the relative probability of the model achieving various lap times. We can also work out statistically the average and best-case lap times that we can expect from the model. I've noticed that the lap times of the model during training resembles a normal distribution, so I use the -2 and -3 Std Dev markers to show the potential best-case lap times for the model, albeit with just 2.275% (-2 SD) and 0.135% (-3 SD) chance of occurring respectively. This helps me to gauge if I should continue cloning and tweaking the model, or abandon it and start afresh with a different approach.
<br>
<br>

## Identifying Potential Model Checkpoints for Race Submission
When training many different models for a race, it is common for racers to ask:
<h2>
  
> "*Which model would give me the highest chance of winning a Virtual Race?*"

</h2>
<br>

To answer that question, I plot the top quartile (p25) lap times vs iterations from the training data, to identify potential model checkpoints for race submission. This scatter plot also allows me to identify potential trade-offs between speed (dots with very fast lap times) and stability (dense cluster of dots for a particular iteration). From the above diagram, I would choose the model checkpoints from the 3 highlighted iterations for race submission.
<br>
<br>

![Scatter Plot of Lap Times per Iteration](/images/log_analysis_blog_laptimeperiteration.png)
<br>
<br>

## Identifying Convergence and Gauging Consistency
As racers gain experience with model training, they'll start paying attention to *Convergence* in their models. Simply put, convergence in the AWS DeepRacer context is when a model is performing close to its best (in terms of average lap progress), and further training may harm its performance or make it *Overfit*, such that it will only do well for that track in a very specific simulation environment, but not in other tracks or in a physical DeepRacer car. That begs the following questions:
<h2>

> "*How do I tell when the model has converged? How consistent is my model after it has converged?*"

</h2>
<br>

To aid in visualising convergence, I overlay the *Entropy* information from the SageMaker policy training logs over the usual plots for Rewards and Progress. The thinking behind this is, as rewards and progress increase, the entropy value should decrease. When rewards and progress plateau, the entropy loss should also flatten out. Hence I use entropy as an additional indicator for convergence.

To gauge the consistency of my model, I also plot the percentage of lap completions per iteration during training. Once the model is capable of completing 100% of laps, the percentage of completed laps should creep up in subsequent iterations, until around the point of convergence, when the percentage value should plateau too.
<br>
<br>

![Scatter Plot of Lap Times per Iteration](/images/log_analysis_blog_entropyandcompletionratio.png)
<br>
<br>

The model training process is probabilistic because the reinforcement learning agent incorporates entropy to explore the environment. To smoothen out the effects of the probablistic model in my visualisation, I use a simple moving average over 3 iterations for each of my plotted metrics.
<br>
<br>

## Identifying Inefficiencies in Driving Behaviour
Once racers have a competitive model, they'll start to wonder:
<h2>

> "*Are there sections of the track where the car is driving inefficiently? What are the sections where I can encourage the car to speed up?*"

</h2>
<br>

In pursuit of answering these questions, I designed a visualisation that shows the average speed and steering angle of the car measured at every waypoint along the track. This allows me to see visually how the model is negotiating the track, because from this plot you can see the rate at which the model is speeding up or slowing down as it travels through the waypoints.
<br>
<br>

![Spain F1 Track Waypoints](/images/log_analysis_blog_spainf1trackwaypoints.png)
<br>
<br>

You can also see how the model is adjusting its steering angle as it negotiates turns. What I love about this visualisation, is that it allows me to see clearly at which point after a long straight is the model starting to "brake", before entering into a turn. It also helps me to visualise if a model is accelerating quickly enough upon exiting a turn.
<br>
<br>

![Speed and Steering per Waypoint](/images/log_analysis_blog_speedsteeringperwaypoint.png)
<br>
<br>

## Identifying Track Sections to Tweak Actions & Rewards
While Speed is the primary performance criteria in a Time Trial race, Stability is also important in an Object Avoidance or Head-to-head race. As time penalties for going off-track have an impact on race position, it is very important to find the right balance between Speed and Stability. Even if the model is able to negotiate the track well, top racers will also be asking these questions:
<h2>

> "*Is the car over- or under-steering at any of the turns? Which turn should I focus on optimising in subsequent experiments?*"

</h2>
<br>

By plotting a heatmap of rewards over the track, it is easy to see how consistently we are rewarding the model at various parts of the track. A thin band in the heatmap reflects very consistent rewards, while a sparse scattering of dots brings to attention the parts of the track where the model is having trouble getting rewards. For my reward function, this usually highlights the turns at which the model is over- or under-steering.
<br>
<br>

![Rewards Heatmap](/images/log_analysis_blog_rewardsheatmap.png)
<br>
<br>

## Experiment, Experiment, Experiment...
For the F1 ProAm Race which ran in the month of May, I planned to do 2 experiments per day (ie., at least 60 experiments), to try out different reward strategies and racing lines. Using Log Analysis to surface insights from the training data, I was able to iterate quickly while focusing on incremental improvements. It helped me to win the race against other top racers and F1 pros, so it is my hope that by sharing these ideas with the community, others can benefit and learn from them too.
<br>
<br>

<h2>

> "*Is the car going flat out on the start / finish straight?*"

</h2>

![Rewards Heatmap for Highest Speed](/images/log_analysis_blog_rewardsheatmaphighestspeed.png)
<br>
<br>

Together as a community of practice, we can help to accelerate learning for everyone, and to raise the bar for the AI/ML community in general.
<br>
<br>

## Cleaning Up
To save on ML compute costs, when you're done with Log Analysis, you can stop the Notebook instance without deleting it. The Notebook, data and log files will still be retained, as long as you don't delete the Notebook instance. Note that a stopped instance will still incur cost for the provisioned ML storage. But you can always re-start the instance later on to continue working on the Notebook.

When you no longer need the Notebook or data, you can permanently delete the instance, which will also delete the attached ML storage volume, so that you'll no longer incur its related ML storage cost.
<br>
<br>
