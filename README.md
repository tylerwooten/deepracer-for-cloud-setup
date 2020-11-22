# JPMC DeepRacer For Cloud Setup

The main purpose of this repo is to document how I was able to set up an EC2 instance through a JPMC sandbox and train model for the AWS DeepRacer competitions. I utilized the [LarsLL deepracer for cloud github repo](https://larsll.github.io/deepracer-for-cloud/) for training. By training on EC2 instances, my team and I have been able to win in the Houston Races, JPMC Global Races, Global Financial Services Races, and we are now competing at AWS reInvent 2020 to try and take home to trophy.

Training on an EC2 has many advantages:
* Being able to set up a customized action space
* Train much faster with up to triple the number of workers on a g4dn.4xl instance
* Ability to increment your training
* Improved log analysis tools
* Reduced cost: $1.25/hour cost of training versus $3.50/hour on amazon console

If you have any issues getting stuck please reach out to Tyler Wooten in slack (https://jpmc-deepracer.slack.com/archives/C01DC150R29) or via email tyler.wooten@jpmchase.com. 
