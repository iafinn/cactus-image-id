# Directions for Running the Notebooks on AWS EC2 Instance

I decided to run this project on a Linux AWS instance to get familiar with this technology. I mostly followed Chris Albon's [directions](https://chrisalbon.com/aws/basics/run_project_jupyter_on_amazon_ec2/) with a few modifications.  Here are the steps I took to set this up.

1. Go to AWS EC2 main page. First you need to create an account, and then go to EC2 in the services menu.

2. Click launch instance from the EC2 main page.

3. Select Amazon Linux 2 AMI (HVM), SSD Volume Type.

4. Select t2.large instance type from menu (costs $0.094/hour, not free but fairly cheap). These notebooks will need a few GB of memory to run so the free tier micros won't cut it.

5. Click next (keeping defaults) until you get to the "Configure Security Group". Here you need to add a few entries:
 - add SSH with TCP protocol and port number 22 with source from anywhere (needed to access instance)
 - add HTTPS with TCP protocol and port number 443 with source from anywhere (needed to access NB)
 - add custom TCP on port 8888 from anywhere (needed to access NB)

7. Select a new key pair name and download the pem key file (I'll call mine ```cactus.pem```)

8. Click launch instance, and go to main EC2 page to wait for the instance to initialize. It usually takes a few minutes.

9. While waiting, change permissions on the pem file ```chmod 400 name_here.pem``` so only root can read.

10. Once your instance is initialized, click connect.

11. Select "I would like to connect with A standalone SSH client."

12. SSH in using the example code from AWS: ```ssh -i "cactus.pem" ec2-user@your-address-here``` On windows you can use PUTTY, or the Anaconda Prompt to SSH. On Linux or Mac you can use the normal terminal.

13. You are now connected to your EC2 instance!

14. Download Anaconda Python onto your instance with ```wget https://repo.anaconda.com/archive/Anaconda3-2019.07-Linux-x86_64.sh``` (may need to update address with newest version of Anaconda for Linux)

15. Install Anaconda with  ```bash Anaconda3-2019.07-Linux-x86_64.sh```

16. Enter, "yes" a few times, and wait for it to be installed and to initialize Anaconda.

17. Many of the EC2 versions come with Python. You'll want to make sure that the PATH variable goes to Anaconda first rather than the original installation: ```export PATH=/home/ec2-user/anaconda3/bin:$PATH```

18. You can check the location of the Python variable with ```which python``` and it should return ```~/anaconda3/bin/python```

19. Install Keras and Tensorflow, because they weren't included with Anaconda: ```pip install Keras``` and ```pip install Tensorflow```

20. Make a notebook directory: ```mkdir notebooks```

21. Back on your own computer, copy the notebook and data files over to EC2 instance using ```scp -i cactus.pem cactus-CNN.ipynb cactus-LR.ipynb data.tar.gz ec2-user@your-address-here:/home/ec2-user/notebooks/```

22. Back on the EC2 instance, set your Jupyter notebook password by running the following commands:

 ```ipython```

```from IPython.lib import passwd```

```passwd()```

Then, enter and verify your password:

```Enter password:``` ```Verify password:```

mark down the long string of characters (the encrypted password):
```'sha1:many-characters-here'```

And finally exit ipython:

```exit```

23. Make a Jupyter config file ```jupyter notebook --generate-config```

24. Setup an HTTPS certificate with the following commands:

```mkdir certs```

```cd certs```

```sudo openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem```

Fill in questions (including email, location, etc.)

25. Navigate to the Jupyter config file and open with vi:

```cd ~/.jupyter/```

```vi jupyter_notebook_config.py```

At the top of the config file add the following lines:

```
c = get_config()

c.IPKernelApp.pylab = 'inline'  #plotting support in the notebook

c.NotebookApp.certfile = u'/home/ec2-user/certs/mycert.pem' #location of your certificate file
c.NotebookApp.ip = '0.0.0.0'
c.NotebookApp.open_browser = False  #no browser by default
c.NotebookApp.password = u'sha1:many-characters-here'  #the encrypted password from above
# Set the port to 8888, the port we set up in the AWS EC2 set-up
c.NotebookApp.port = 8888
```
then exit and save with ```:wq```

26. Make a new screen with ```screen``` and go to the notebooks folder with ```cd```

27. Start jupyter notebooks with ```jupyter notebooks``` and detach with ```ctrl-a d```

28. Go back to your home computer and open a browser. set the URL to https://your-address-here:8888

29. Jupyter notebooks should open in the browser and ask for your password. Enter the password and now you can work on the notebook from your home computer!
