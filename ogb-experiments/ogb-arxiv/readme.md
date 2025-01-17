# OGB-Arxiv Dataset Experiment

In this folder, you may conduct experience on OGB-Arxiv dataset for 4 experience settings:

1. subgroups by aggregated-feature distance

   a. With original features

   b. With noisy features

2. Subgroup by geodesic distance

3. Subgroup by node centrality

## Quick Start

0. Cache the noisy version of data

```bash
python generate_noisy_data.py
```

1. Cache the subgroups

```bash
# cache for aggregated-feature subgroup
python agg_subgroup.py  # aggregated-feature distance on original features

python agg_subgroup.py --noise_on_feature  # aggregated-feature distance on noisy features

# cache for node centrality subgroup
python degree_subgroup.py
python pagerank_subgroup.py

# cache for geodesic distance subgroup
python geodesic_matrix.py && python geodesic_subgroup.py
```
**Notes**:
- ~~`python agg_subgroup.py`~~ Out of memory: agg = torch.sparse.mm(agg, A).to_dense().numpy()
- ~~`python agg_subgroup.py --noise_on_feature`~~ Out of memory: agg = torch.sparse.mm(agg, A).to_dense().numpy()
- ~~`python geodesic_matrix.py && python geodesic_subgroup.py`~~ Took a long time (9:54:31.37), thrashed a lot (page faults: 35751836, context switches: 1400962), and, eventually ran out of memory (virtual memory size: 119.53 GB, real memory size: 2.18 GB) on a 128GB main-memory EC2 instance.


2. Train the models

```bash
# Training on original features
python gnn.py  # GCN
# 169 epochs in 35m on MacBook Pro
python gnn.py --use_sage  # SAGE
python mlp.py  # MLP

# Training on noisy features
python gnn.py --noise_on_feature  # GCN
python gnn.py --use_sage --noise_on_feature  # SAGE
python mlp.py --noise_on_feature  # MLP
```

3. Print the result

```bash
# GCN and SAGE
python res.py --type agg # GCN, subgroup by aggregated-feature distance on original features
python res.py --type agg --noise_on_feature  # MLP, subgroup by aggregated-feature distance on noisy features
python res.py --use_sage --type agg  # SAGE, subgroup by aggregated-feature distance on original features
python res.py --use_sage --type agg --noise_on_feature  # SAGE, subgroup by aggregated-feature distance on noisy features
python res.py --type geodesic_distance # GCN, subgroup by geodesic distance
python res.py --use_sage --type geodesic_distance # SAGE, subgroup by geodesic distance
python res.py --type degree # GCN, subgroup by degree centrality
python res.py --use_sage --type degree # SAGE, subgroup by degree centrality
python res.py --type pagerank # GCN, subgroup by pagerank centrality
python res.py --use_sage --type pagerank # SAGE, subgroup by pagerank centrality

# MLP
python res-mlp.py  # MLP, subgroup by aggregated-feature distance on original features
python res-mlp.py --noise_on_feature  # MLP, subgroup by aggregated-feature distance on noisy features
```

