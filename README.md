# PANINI CLUSTER

Building upon the [ALBERT training tutorial](https://github.com/learning-at-home/hivemind/tree/master/examples/albert) in the [Hivemind](https://github.com/learning-at-home/hivemind) GitHub repository.

## Prerequisites

- Download the Project 
  ```
  wget https://code.swecha.org/swecha-sites/panini-cluster.git
  ```

- (Optional, recommended) Create a virtual environment and activate it

  ``` sh
  python3 -m venv ~/panini-venv
  source ~/panini-venv/bin/activate
  ```

- Install the requirements using `requirements.txt`
  ```
  pip3 install -r requirements.txt
  ```

- Preprocess the data
  ```
  python3 tokenize_wikitext103.py
  ```


## Running an experiment

There are two kinds of nodes you can run.

1. Training Monitor: Tracks the training progress and provides a multiaddress to allow other peers to join the network. At least one training monitor must be up at all times. You can have multiple training monitors running at any given time.

2. Trainer nodes: Connects to an existing node in the network and contributes to model training. Most of the nodes in the P2P network must be trainer nodes.

### First peer in the network is a training monitor

The `run_training_monitor.py` file starts a *Training monitor* that keeps track of the training process and allows other peers to connect. Preferably, the training monitor should be running on a machine with a public IP address, so that other peers can easily discover the first peer. Ensure that WandB has been set up before starting the training monitor (if you're new to WandB, go through the [WandB Quickstart](https://docs.wandb.ai/quickstart) to understand how to set it up).

Run the training monitor using the following command.

``` sh
python3 run_training_monitor.py --wandb_project 2023-12-29-albert --host_maddrs /ip4/0.0.0.0/tcp/31337 --identity_path ./identity.key
```

This command runs the training monitor on the TCP port 31337 and read the identity from `identity.key` (if `identity.key` doesn't exist, then this command creates it). All the training logs are saved in the project named *2023-12-29-albert*.

### Run a trainer node

The `run_trainer.py` starts a trainer node, which start the actual training process. You can customize the training process by providing argument to the command. To see the full list of commands, run `python3 run_trainer.py --help`. The following are some useful arguments

- `--initial_peers MULTIADDRESS`: Specify which peers in the network you wish to connect to. Typically, you would specify the multiaddress of a training monitor (see example below). However, you can specify the multiaddress of any node in the P2P network.

- `--per_device_train_batch_size NUM`: Number of samples that are processed by the trainer node in a single batch. Set `NUM` to a small number if the machine doesn't have enough comp0ute power.

- `--use_cpu BOOL`: Determines whether to use CPU or not. Set `BOOL` to `True` if CPU training is desired. Also use `--no-fp16` if CPU training is desired, to disable 16-bit floating point training.

- `--no_fp16`: Disables 16-bit floating point training.

For example, run the following script to start a CPU-only training node that connects to a peer whose multiaddress is `/ip4/65.108.151.120/tcp/31337/p2p/QmRY4WjaPWw2q1x9Ciqc4efXqGfPhrugUc1FMCtMBPY7VB`.
``` sh
python3 run_trainer.py --initial_peers /ip4/65.108.151.120/tcp/31337/p2p/QmRY4WjaPWw2q1x9Ciqc4efXqGfPhrugUc1FMCtMBPY7VB --use_cpu True --no_fp16
```

To run a training node with GPU, you can simply omit the flags at the end and run the following.

``` sh
python3 run_trainer.py --initial_peers /ip4/65.108.151.120/tcp/31337/p2p/QmRY4WjaPWw2q1x9Ciqc4efXqGfPhrugUc1FMCtMBPY7VB
```

- If you are not using WandB, then choose option `3. Don't visualize my results` when prompted and press `Enter`.

    ![Screenshot_from_2023-12-28_18-55-05](/uploads/7dfd13f26fdcde297822a56c8c22c496/Screenshot_from_2023-12-28_18-55-05.png)

- Working Model Output:

    ![Screenshot_from_2023-12-28_18-57-15](/uploads/98f28d5697b5f3a83de6fe8738cbe1d7/Screenshot_from_2023-12-28_18-57-15.png)
