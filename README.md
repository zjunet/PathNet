# Beyond Homophily: Structure-aware Path Aggregation Graph Neural Network
## PathNet
![PathNet](./Figs_in_paper/PathNet.png)

## File folders

`splits`: need to unzipped, contains the split data of "Cora, Cornell, Pubmed and Citeseer".

`other_data`: need to unzipped, contains the data of "Nba, Electronics, Bgp". (The splits are random generated in `run.py`)

`baseline`: contains baseline models.

`draw_figs`: contains the code to draw the results' figs in the paper.

`edge_input`: contains the edge files used in preprocessing.

`preprocess`: contains the preprocessing code.

`synthetic_data`: contains the code for generate the labels of synthetic experiments.

## Path Sampler (preprocessing)

This part can be done before training.
To generate the paths for dataset *data_name* (*eg.* Cora). In *gen.cpp*, we change the names of the input and output files to *data_name*, then compile and run *gen.cpp*. The program will generate a file containing all paths.

Compile and run *gen.cpp*  for normal datasets.

```shell
g++ gen_merw.cpp -o gen_merw -g -Wall -O2 -mcmodel=medium
./gen [data_name]
```

*Note:* Fill in the data's name at the [data_name]. For example, when using the Cora dataset, `./gen Cora`. Remember the data_name has to be the same as the file name in the `edge_input` without the postfix`.in`.

If a graph contains too many nodes, the file generated by the above method will be too large and occupy a large running memory. So we provide *gen_epoch.cpp* to generate corresponding paths for each epoch.

Compile and run *gen_epoch.cpp* for large datasets.

```shell
g++ gen_epoch_merw.cpp -o gen_epoch_merw -g -Wall -O2 -mcmodel=medium
./gen_epoch [data_name]
```
## Main model

### Files
`dataset.json`: contains the path of split datasets.

`save_json.ipynb`: Adjust the paths  in `dataset.json`. (Optional)

`dataset.py`: data usage code (imported in run.py ).

`PathNet_run.py`: main code.

### model usage
```shell
python PathNet_run.py -data=cornell
```
Other hyperparameters can be delivered by command line arguments, such as ```-lr=0.005``` and `-data=citeseer`. Other command line arguments can be found in `run.py`. The trained model will be saved in 'saved_models' and the performance will be saved in 'results'.

### Model variants & parameters
Changing the PAGG class can deliver other variants. If you want to try other settings like path length or number of paths, you can first generate the according paths and change the hyperparameters.

## Synthetic experiments
Synthetic graph generators are used to complete the synthesis experiment. At the beginning, all nodes with odd index and even index on the graph are attached with two different features. Synthetic label generators will search all paths with length k for each node (excluding the starting node), and there will be various path patterns. The index of the most path pattern will be selected as the label of the node.

We provide a synthetic label generator for Cora and Citeseer. The two generators will get a file with the same number of lines as the number of nodes in the original network, and each line represents the label of this node in the synthetic graph.

syn_cora:
Compile and run *gen_cora_syn.cpp*

```shell
g++ gen_cora_syn.cpp -o gen_cora_syn -g -Wall -O2
./gen_cora_syn
```

syn_citeseer:
Compile and run *gen_citeseer_syn.cpp*

```shell
g++ gen_citeseer_syn.cpp -o gen_citeseer_syn -g -Wall -O2
./gen_citeseer_syn
```
