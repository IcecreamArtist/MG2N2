# MG2N2

The model is composed of three Graph Neural Network modules and generates the molecular graphs with a sequential algorithm.
A First-In-First-Out node expansion queue keeps track of the nodes to be expanded. 
The node generator module decides if to generate a new neighbor for the node currently under focus and, in case, its type.
The edge classifier decides the type of edge connecting the focused node to its new neighbor.
The linker module decides if any other edge should be added between the new node and the rest of the graph.
The generative process is stopped when the expansion queue becomes empty and/or the maximum graph size is reached. 

# MG2N2 with Attention Enhancement

This repository is an enhanced fork of the original MG2N2 project, extending the Molecular Generative Graph Neural Networks with an attention mechanism. The addition of the attention mechanism to the base model has shown to improve the performance of generating small molecular graphs, which represent the structural formulas of small organic molecules.

## Enhanced Model Overview

The original MG2N2 model utilizes a sequential algorithm and three Graph Neural Network modules to generate molecular graphs. In this enhanced version, we integrate an attention mechanism to refine the node generator module, which has improved the model's ability to focus on relevant features and increased the accuracy of molecule generation.

- **Node Generator**: Decides whether to generate a new neighbor for the current node.
- **Edge Classifier with Attention**: Now determines the type of edge that connects the node under an improved focus to its new neighbor and relevant contextual information.
- **Linker Module**: Decides on the addition of any other edge between the new node and the rest of the graph.

The generative process terminates once the node expansion queue is empty or the maximum graph size is reached.

# requirements
To run the code, you will need a python>2.6 distribution with the following python packages:  
Tensorflow  
Numpy  
Scipy  
Matplotlib  
Networkx  
We used the following package versions:  
numpy==1.19.2  
scipy==1.5.2  
matplotlib==3.3.2  
networkx==2.5  
tensorflow==1.14.0  

# installation
No installation procedure is required, this repository is ready-to-use.

# data setup
QM9 data is provided in a "raw" format, to save space and downloading time. The script "translate_dataset.py" will process the dataset into the right format. The code can be easily adapted to be used on other datasets by substituting the dictionaries that map atom symbols and chemical bond categories to numerical labels, and adapting the input/output dimensions of the neural networks, in accordance to the atom symbols and bond categories found in the dataset under analysis.

# utility scripts
The "graph_decomposition.py" and "molecule_drawer.py" python files contain utilities for the other scripts. In particular, "molecular_drawer.py" provides some routines for sketching molecular graphs with networkx, mainly for debugging purposes. For high-quality images, we suggest drawing the molecules directly with Rdkit. "graph_decomposition.py" provides a set of functions to renumber the nodes with the algorithm described in the the paper, to extract the generation sequence of graphs from each molecular graph, and other related utilities. Please note that these scripts also require a little adaptation when switching from one dataset to another (for instance, the atom symbol -> one-hot vector mapping should be manually specified).

# network training
The script "train_generator.py" trains the generator module.
The script "train_bond_classifier.py" trains the edge classifier module.
The script "train_linker.py" trains the extra edge generator module.
The hyparparameters of each module are declared in the first part of the corresponding script. The trained modules are saved into a "Temp/Modules" folder.

Example:
To train a bond classifier, simply run:
```cmd
python train_bond_classifier.py test2 full
```

# graph preprocessing
The graph preprocessing operations are computationally demanding and take some time to complete. Thus, two operational modes ("short" / "full") are provided for each training script. When running in "full" mode, all the preprocessing operations will be executed, and the preprocessed data will be saved in the "Temp/Batches" folder, in the form of ready-to-load batches of training/validation/test graphs. When running in "short" mode, the script will skip the preprocessing operations and load previously compiled batch files. This means that preprocessing operations need to be carried out only during the first run of each script, or when a parameter which has an effect on preprocessing is changed. Each GNN module has its own subfolder for saving batch files.

# graph generation
The script "generate_graphs.py" generates a batch of graphs, exploiting the three modules which had been previously saved in the "Temp/Modules" folder.
The hyperparameters of the modules are declared at the beginning of the script. At least one trained instance of each module is required to run this script.


