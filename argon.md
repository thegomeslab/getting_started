# Table of Contents
1. [Getting an Account on Argon](#getting-an-account-on-argon)
2. [Accessing Argon](#accessing-argon)
3. [Job Submission](#job-submission)
4. [Port Forwarding on Argon](#port-forwarding-on-argon)

# Getting an Account on Argon

1. Familiarize yourself with the Argon documentation
2. Take the quiz
3. Submit a workflow request

# Accessing Argon

1. Choose your command line application
2. Using SSH to access Argon
3. Accessing the Argon filesystem on your local computer

# Job Submission

Basic job submission: https://wiki.uiowa.edu/display/hpcdocs/Basic+Job+Submission

Advanced job submission: https://wiki.uiowa.edu/display/hpcdocs/Advanced+Job+Submission

QLogin: https://wiki.uiowa.edu/display/hpcdocs/Qlogin+for+Interactive+Sessions

Queues:
-	UI: 5 jobs max, no wall time limit
-	UI-HM: 1 job max, no wall time limit
-	COE: 3 jobs max, no wall time limit
-	UI-GPU: 1 job max, no wall time limit
-	JG: 

Parallel Environments: `smp`, `24cpn`, `32cpn`, `56cpn`, `64cpn`

Configuration:
-	Queue: `-q`, which queue to submit to?
-	Parallel Environment: `-pe`, how are CPUs from cluster allocated? How many CPUs?
-	Wall time: `-l h_rt=`, maximum run time
-	NGPUs: `-l ngpus=`, number of GPUs requested

Submitting Interactive Jobs:
-	Start screen: `screen` or `screen -x`
-	Request resources: `qlogin …` (see below)
-	Once allocated, start conda environment: `conda activate deepchem`
-	Run calculations: `python hopv_script.py`
-	Once calculations are complete, exit the interactive job and go back to the login node: `exit`

Common examples:
-	`qlogin -q UI-GPU -l ngpus=1 -l h_rt=24:00:00`: Request 1 GPU for 24 hours
-	`qlogin -q UI -pe 56cpn 56 -l h_rt=48:00:00`: Request 1 56 CPU node for 48 hours

# Port Forwarding on Argon
Jupyter notebooks are nice in that they give a file manager and persistant calculation environment that is accessible from a web browser. The bad news is that, in order to access it on your local computer while running on Argon, you must forward the display over SSH from argon to your computer using port forwarding. This is further complicated by the fact you will likely be running the notebooks on a compute node, and so an extra step of port forwarding is required. This is probably easier than it sounds, and you’ll get used to it after running a few times. Here’s the steps:
1.	Make sure to have jupyter installed (`conda install jupyter`)
2. Pick a port to forward over and use it consistently. For this example we’ll use 1111
3.	Make a forwarding connection between your laptop and a specific Argon login node. Use the command `ssh -L 1111:localhost:1111 -p 40 hawkid@argon-login-1.hpc.uiowa.edu`
4.	Create/re-join a screen `screen -x` or `screen`
5.	Once inside the screen, get a CPU/GPU node depending on what you want to do `qlogin -q UI …` and note the compute node address that you join. For example, “argon-itf-bx47-22”
6.	On the compute node, activate your conda environment and run a jupyter notebook instance from your home directory `conda activate ax` `jupyter notebook --no-browser --port=1111`. It should print some text to the screen like “The Jupyter notebook is running at `http://localhost:1111/...`. Copy that entire address.`
7.	Create a new window within the screen `C-a c`
8.	Make a forwarding connection between the login node (where you started your screen) and the computer note. Make sure to run this command from the same node that you logged into in step 2 `ssh -L 1111:localhost:1111 hawkid@argon-itf-bx47-22`
9.	Copy the address `http://localhost:1111/...` into a web browser on your laptop. You should see a list of files that looks like your home directory.
10.	Navigate to the location of the ipynb file and click to open it.
11.	You can run individual computation cell by clicking on it and pressing “Shift+Enter” or run the entire notebook as you would a script by using the menu at the top of the screen “Kernel > Restart and Run All”.

Now, for some simple models it should be fine to run Jupyter on a login node. Then, you would run steps 1,2, 5, and 8. Perhaps the first time you do this, just run those steps just to see how it goes. Once you run through the entire set of commands, close your computer, and want to come back, you should be able to just repeat steps 1-3 and 8 assuming that your screen is still open and the compute node job you started is also running.
