To run Jupyter Notebooks on Argon you must forward the display over SSH from Argon to your computer using port forwarding. Additionally, if you are running notebooks on a compute node a
second step of port forwarding is required. To run Jupyter notebooks on Argon:

1.	Make sure to have jupyter install (`conda install jupyter`)
2.	Pick a port to forward over and use it consistently. For this example we’ll use 9876
3.	Make a forwarding connection between your laptop and a specific Argon login node. Use the command `ssh -L 9876:localhost:9876 -p 40 hawkid@argon-login-1.hpc.uiowa.edu`
4.	Create/re-join a screen `screen -x` or `screen`
5.	Once inside the screen, get a CPU/GPU node depending on what you want to do `qlogin -q UI …` and note the compute node address that you join. For example, `argon-itf-bx47-22`
6.	On the compute node, activate your conda environment and run a jupyter notebook instance from your home directory `conda activate dc; jupyter notebook --no-browser --port=9876`. It should print some text to the screen like “The Jupyter notebook is running at http://localhost:9876/...”. Copy that entire address.
7.	Create a new window within the screen `C-a c`
8.	Make a forwarding connection between the login node (where you started your screen) and the computer note. Make sure to run this command from the same node that you logged into in step 2 `ssh -L 9876:localhost:9876 hawkid@argon-itf-bx47-22`
9.	Copy the address “http://localhost:9876/...” into a web browser on your laptop. You should see a list of files that looks like your home directory.
10.	Navigate to the location of the ipynb file and click to open it.
11.	You can run individual computation cell by clicking on it and pressing `Shift+Enter` or run the entire notebook as you would a script by using the menu at the top of the screen `Kernel > Restart and Run All`.

Simple models can be run on the login node, to run a notebook on the login node only do steps 1,2,5, and 8.

Once you run through the entire set of commands, close your computer, and want to come back, you should be able to just repeat steps 1-3 and 8 assuming that your screen is still open and the compute node job you started is also running.

Converting Jupyter Notebooks to .py:
`jupyter nbconvert --to python filename.ipynb`
