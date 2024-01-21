# PANINI CLUSTER

## Prerequisites

- Clone the Project 
  ```
  git clone https://code.swecha.org/Cherukupalle_Naveen/panini-cluster.git
  ```

- (Optional, recommended) Create a virtual environment and activate it

  ``` sh
  python3 -m venv ~/petals-venv
  source ~/petals-venv/bin/activate
  ```

- Install the requirements
  ```
  pip3 install git+https://github.com/bigscience-workshop/petals
  ```


## Running an experiment

There are two kinds of nodes you can run.

1. Training Monitor: Tracks the training progress and provides a multiaddress to allow other peers to join the network. At least one training monitor must be up at all times. You can have multiple training monitors running at any given time.

2. Trainer nodes: Connects to an existing node in the network and contributes to model training. Most of the nodes in the P2P network must be trainer nodes.

### To join your device as a peer into the inferencing network,
python3 -m petals.cli.run_server bigscience/bloom-560m --initial_peers /ip4/65.108.151.120/tcp/31337/p2p/QmbPdbE4bsjPGEV7KDepxPFBqmRtQuK479LVi8qvB8seL1 --torch_dtype float32 --num_blocks 10




## Resources
Resources:
https://github.com/bigscience-workshop/petals
https://github.com/petals-infra/chat.petals.dev
https://github.com/petals-infra/health.petals.dev