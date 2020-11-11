Basic job submission: https://wiki.uiowa.edu/display/hpcdocs/Basic+Job+Submission
Advanced job submission: https://wiki.uiowa.edu/display/hpcdocs/Advanced+Job+Submission
QLogin: https://wiki.uiowa.edu/display/hpcdocs/Qlogin+for+Interactive+Sessions

Queues:
•	UI: 5 jobs max, no wall time limit
•	UI-HM: 1 job max, no wall time limit
•	COE: 3 jobs max, no wall time limit
•	UI-GPU: 1 job max, no wall time limit
•	JG: 

Parallel Environments: “smp”, “24cpn”, “32cpn”, “56cpn”, “64cpn”

Configuration:
•	Queue: “-q”, which queue to submit to?
•	Parallel Environment: “-pe”, how are CPUs from cluster allocated? How many CPUs?
•	Wall time: “-l h_rt=”, maximum run time
•	NGPUs: “-l ngpus=”, number of GPUs requested

Submitting Interactive Jobs:
•	Start screen: “screen” or “screen -x”
•	Request resources: “qlogin …” (see below)
•	Once allocated, start conda environment: “conda activate deepchem”
•	Run calculations: “python hopv_script.py”
•	Once calculations are complete, exit the interactive job and go back to the login node: “exit”

Common examples:
•	“qlogin -q UI-GPU -l ngpus=1 -l h_rt=24:00:00”: Request 1 GPU for 24 hours
•	“qlogin -q UI -pe 56cpn 56 -l h_rt=48:00:00”: Request 1 56 CPU node for 48 hours
