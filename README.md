# deepracer-for-cloud-setup

The main purpose of this repo is to document how I was able to set up an EC2 instance through a JPMC sandbox and train model for the AWS DeepRacer competitions. I utilized the [LarsLL deepracer for cloud github repo](https://larsll.github.io/deepracer-for-cloud/) for training. 

Training on an EC2 has many advantages:
* Being able to set up a customized action space
* Train much faster with up to triple the number of workers on a g4dn.4xl instance
* Ability to increment your training
* Improved log analysis tools

If you have any issues getting stuck please reach out to Tyler Wooten in slack (https://jpmc-deepracer.slack.com/archives/C01DC150R29) or via email tyler.wooten@jpmchase.com. 
