# CoCoLab Guide to CCN Cluster Setup

A short walkthrough common questions and setup for getting access to the CCN (GPU) cluster. A lot of information here will repeated from https://github.com/neuroailab/cluster_setup/wiki/CCN-Cluster.

## Current CoCoLab CCN users

```
shyamal, wumike, bpeloqui, bria, malikali, schopra8, btorok, rxdh, anie, erindb
```

All other users have lower priority on Node10/13.

## Getting Access

Email wumike@stanford.edu (your CoCoLab contact for cluster questions) to have him request an account for you with Dan Yamins (the cluster owner). Please provide the following information:

```
Full Name e.g. Mike Wu
Email e.g. wumike@stanford.edu
Requested Account Username e.g. wumike
```

Dan will likely send you an email joining the CCN Slack. This is where cluster admin post information about bugs/broken nodes/updates/etc. **Once you successfully join the slack**, Dan will create an account for you.

Your password will be set to:

```
hello<username>
```

## Using the Cluster

To login to the cluster, use the following:

```
ssh <username>@nodeX-ccncluster.stanford.edu
```

The cluster is partitioned across many labs and CoCoLab has reserved the entirety of node13. If possible, you should use the GPUs on **node13**. However, you are free to use any other node but may get kicked off by the reserved party.

Also, node1 is special as it is the master node. To run `salt` commands (i.e. change you password on all nodes), go there.

**We have sudo access** so be careful! In general this is helpful to grant yourself permissions but don't delete other peoples files.

### Helpful Commands

To see the GPUs and the processes being run, call `nvidia-smi` in bash. You will see 10 Titan X's with the amount of memory being used on each one. In total, 1 Titan X has a little under 12000Mi memory.

To see who a process id belongs to, run `ps aux | grep <pid>`. This will give you their username. Another helpful command is `htop`.

### Running jobs

We do not use slurm meaning you can run things directly. I personally like to use a `screen` so that processes don't die if I close the terminal (`tmux` is similar). You can specify a particular GPU by

```
CUDA_VISIBLE_DEVICES=0 <rest_of_command>
```

In general it is a good idea to specify the GPU so you minimize clash with other people's jobs.

### Job Priority

When you find someone else (who does not have priority on node13 i.e. not in CoCoLab), you are able to kill their job to run yours. It is however, polite to first chat them on the CCN slack and let them know. To kill a job, run:

```
sudo kill <pid>
```

Again, be careful! Also, if you are running jobs on other nodes, be aware that your jobs may be killed.


### Installing Things

Because you have `sudo`, you can install whatever you want. **But this is generally not a good idea** as versions will conflict with others. Default libraries have been installed like PyTorch, Tensorflow, NumPy so you will automatically have them. If (like me) you need specific versions of libraries or new ones, I have a Miniconda installer sitting in `/mnt/fs5/wumike/`. This will install a local virtual environment that should be easy to manage. Once you install an environment, call `source activate <env_name>` to activate it. For example, this is my workflow:

```
sudo cp /mnt/fs5/wumike/Miniconda2-latest-Linux-x86_64.sh ~
cd ~
bash Miniconda2-latest-Linux-x86_64.sh
conda create -n <env_name> python anaconda
source activate <env_name>
conda install <whatever_you_want>
pip install <whatever_you_want>
```

Remember you activate your environment or else it's like you didn't do anything. To deactivate, run `source deactivate`.

### Storage

There is a lot of storage. You should **not** save your files in your personal `~` directory. Instead, check out `/mnt/fsX` where `X` is greater than 2. These are huge filesystems and allow for fast read and write (see CCN wiki for why). You can `sudo mkdir` a directory in one these for yourself. For example, I have one in `/mnt/fs5/wumike`. You can store datasets, trained models, etc. here. There are also some common datasets (i.e. ImageNet) already loaded in each filesystem.


