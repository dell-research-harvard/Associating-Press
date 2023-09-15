# Associating Press

<img width="600" alt="image" src="https://github.com/dell-research-harvard/Associating-Press/assets/86472495/33dadd57-29a3-471f-a714-5a7e09fc9c30">


## Task
Articles in newspapers and other print media often span multiple columns and have complex layouts. 

Object detection models 

<img width="554" alt="image" src="https://github.com/dell-research-harvard/Associating-Press/assets/86472495/dfcf1723-74ac-4060-aaf2-f05740f2f1ff">


## Labelled Data 

We evaluate these methods on a hand-labeled dataset of 214 scans, randomly selected from 1968 and 1955. These scans were labeled by a highly-trained undergraduate research assistant. Summary statistics of this dataset are given in table \ref{tab_aa_dat} and evaluation results are given in the main text. 


## Outline of Approach

<img width="548" alt="image" src="https://github.com/dell-research-harvard/Associating-Press/assets/86472495/044304b8-4ccb-4e18-9891-2b0ae1cb1bc2">


### Rule-Based Algorithm

First, we used a rule-based algorithm using associate article bounding boxes that are under the same headline, as these are part of the same article with extremely high probability. Algorithm \ref{rule_algo} gives pseudocode for this method. We set the parameters as $P_S = 100$, $P_T = 20$, $P_B = 50$.

For training data, where we want article pairs that are not only part of the same article, but also where they appear in the given order, we further narrow down the pairs. Specifically, we use only those pairs which are horizontally next to each other, and which have no other bounding boxes below them, as for these pairs, we can guarantee that the pair of bounding follow directly after one another (whereas for other article bounding boxes that share a headline, there may be a third bounding box in between). Algorithm \ref{order_algo} shows pseudocode for this procedure, and we used $P_C = 5$, and it is further illustrated in panel A of figure 6 in the main text. 

For hard negatives, we used article boxes under the same headline in reverse reading order (right to left). For standard negatives, we took pairs of articles on the same page, where B was above and to the left of A, as articles do not read from right to left. One twelfth of our training data were positive pairs, another twelfth were hard negative pairs and the remainder were standard negative pairs. This outperformed a more balanced training sample. 

### Cross-encoder 

We use this dataset to finetune a cross-encoder using a RoBERTa base model \cite{liu2019roberta}. We used a Bayesian search algorithm \cite{pmlr-v80-falkner18a} to find optimal hyperparameters on one tenth of our training data (limited compute prevented us from running this search with the full dataset), which led to a learning rate of 1.7e-5, with a batch size of 64 and 29.57\% warm up. We trained for 26 epochs with an AdamW optimizer, and optimize a binary cross-entropy loss.

## More information 

If you find this work useful, please cite the following paper:
