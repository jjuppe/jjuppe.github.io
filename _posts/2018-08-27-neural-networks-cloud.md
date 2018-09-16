---
layout: post
title: How to train a Neural Network in the Cloud?
tags: [Cloud computing, Amazon EC2, Screen, Python, GPU computing]
feature-img: "assets/img/AWS.jpg"
excerpt_separator: <!--more-->
---

Neural Networks need a hell lot of computational power! This is what I realized after I tried training my first Neural Network on my CPU.
<!--more--> Therefore, I was kind of forced to look for viable options and stumbled upon Amazon's EC2 instances. In this post I will show you how to set-up an Amazon web instance for computationally expensive jobs like Machine Learning or Simulations.     
# Setting up an Amazon EC2 instance
There are many reasons to set up an Amazon EC2 instance - for me it was to train a complex neural network with over 20 million parameters. The process to launch an instance is quite straight-forward. 
To get started you need to create an account with Amazon Web Services. Once you have done this, move to the *Management Console*. In the Management Console please select *EC2* under *Compute*. 

![Management Console]({{%site.baseurl%}}/assets/img/Management-Console.png)

Then, click on *Launch Instance* to launch an instance. 

In this tutorial I will select an instance without any pre-installed packages: Ubuntu Server 16.04 LTS with ami-0552e3455b9bc8d50 (ami= Amazon Machine Image).

Next, you can decide which kind of instance you would like to launch. Usually, if you are planning to train a neural network you would filter for GPU compute instances and go with one of the p instances. For an explanation of the differences between p2 and p3 instances see this [post](https://blog.iron.io/aws-p2-vs-p3-instances/). However, for this tutorial we will stick to a t2.micro instance since they offer *free tier eligibility*.

Select your preferred instance type and click on *Next: Configure Instance Details*. You can leave these settings as they are. Click on *Next: Add Storage* and select a sufficient storage size for your project. Make sure to select enough storage as it is quite hard to increase this number later on when you realize that you are storage is full (been there, done that...). Btw: to check your disk space usage simply type *df*. 

Skip the *Add tags* and move on to *Configure Security Group*. Here you can define a set of firewall rules that control traffic to your instance. What has worked for me thus far (admittedly not the safest option) is to add a new TCP Rule like this:

![Security Rules]({{% site.baseurl %}}/assets/img/AWS-Security-Rule.png)

Like this you will be able to access port 8888 from your local computer. I have used this for Jupyter Notebooks or to have a look at your training with tensorboard. It works just fine. 

Afterwards, you just review the settings and and can click on launch. In order to access your instance you will need a key. AWS stores one version of that key and you also have to download a .pem key. Make sure you don't loose that key in the depth of your computer, you will need it to ssh into your cloud instance. 

After you have launched your instance, you can go to your instance overview to see which instances are running at the moment. 

![Instance Overview]({{%site.baseurl%}}/assets/img/AWS-Instance-overview.png)

If you click on *Connect* directions and terminal commands to ssh into your instance are given. The easiest way is to cd to your *.pem file and then run:

```shell
ssh -i "myaws.pem" ec2-instance@ec2-xx-xxx-xx-xxx.us-east-2.compute.amazonaws.com
```

Once you see this screen, you have connected to your instance:

```shell
Welcome to Ubuntu 16.04.4 LTS (GNU/Linux 4.4.0-1066-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

31 packages can be updated.
0 updates are security updates.


Last login: Thu Aug 23 20:01:20 2018 from xx.xxx.xx.xx
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@ip-xxx-xx-xx-xxx:~$ 
```

# Running a python script 
To actually be able to run something you need to transfer data to your cloud instance. To do this, also cd to your *.pem file and write the following command:
```shell
scp -i /path/my-key-pair.pem /path/SampleFile.txt ec2-user@c2-198-51-100-1.compute-1.amazonaws.com:/home/ubuntu/mypython/
```

Replace the path with the correct file and replace the *ec2-user* with the correct instance type. Choose the correct one from the following table:  

| AMI instance                       | user name          |
| :----------                         | :-------------:    |
| Amazon Linux 2 or Amazon Linux AMI | ec2-user           |
| Centos                             | centos             |
| Debian AMI                         | admin or root      |
| Fedora AMI                         | ec2-user or fedora |
| RHEL AMI                           | ec2-user or root   |
| SUSE AMI                           | ec2-user or root   |
| Ubuntu AMI                         | ubuntu             |

Then, in order to run the script we will install the Anaconda Distribution. To learn more about Anaconda see this [post.]({{site.baseurl}}/2018/08/21/anaconda.html)

First, make a temporary folder:
```shell
mkdir tmp
```

Then, we will download the Anaconda file using *curl*:
```shell
curl -O https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86_64.sh
```

Once the download is finished we have to run the script:
```shell
bash Anaconda3-5.0.1-Linux-x86_64.sh
```

In order to activate the installation, type the following:
```shell
source ~/.bashrc
```

Check if the installation was succesfull by typing:
```shell
which python
/home/ubuntu/anaconda3/bin/python
```

Now that we have a functioning Python interpreter we can cd back to the folder of our python file. In the folder we can then run the script as you would normally do. 

Congratulations, you have run your fist script on your EC2 instance. 

With these tools you can run the training of your NN in the Cloud and enjoy the computing power Amazon and the likes provide at your disposal. 

# Using screen
Another thing I want to share with you is screen. It is one of the best discoveries that I made while learning about cloud computing. 

Screen is a full screen software program that allows you to run multiple terminal sessions inside a single terminal window manager and is usually already pre-installed on Ubuntu. This tool is very useful if you want to run different programs in different terminal shells so the output does not get mixed. Also, you can start a session to run your neural network and detach and reattach to this session as you like. This means the running of your script is not bound to your active ssh connection anymore. You start your training and  detach from the session. To check the progress of the training you simply reconnect to your instance and re-attach to the session.

See this [post](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-screen-on-an-ubuntu-cloud-server) to learn more about screen commands. 

What is your favorite cloud hosting provider and on which setup do you train your neural networks?

