On Argon
1.	Get GPU and setup environment
  •	qlogin -q UI-GPU
  •	module load cuda
2.	Install pre-req.’s (create a new environment if needed)
  •	conda create -n deepchem python=3.7
  •	conda activate deepchem
  •	conda install -c deepchem -c rdkit -c conda-forge -c omnia deepchem-gpu=2.3.0
  •	conda install pytorch==1.2.0 torchvision cudatoolkit=9.2 -c pytorch
3.	Install pytorch_geometric https://github.com/rusty1s/pytorch_geometric
  •	pip install --verbose --no-cache-dir torch-scatter==1.4.0
  •	pip install --verbose --no-cache-dir torch-sparse==0.4.3
  •	pip install --verbose --no-cache-dir torch-cluster==1.4.5
  •	pip install --verbose --no-cache-dir torch-spline-conv
  •	pip install torch-geometric==1.3.2
4.	Clone graph_moleculenet
  •	git clone https://github.com/joegomes/graph_moleculenet.git
5.	Install graph_moleculenet
  •	cd graph_moleculenet; python setup.py develop
6.	Run an example
  •	cd graph_moleculenet/examples/tox21
  •	python tox21_gin.py
