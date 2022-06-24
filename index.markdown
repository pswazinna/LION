---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---
{: style="text-align:center"}

Phillip Swazinna    Steffen Udluft    Thomas Runkler

[Read the Full Paper](https://arxiv.org/abs/2205.10629)

Offline reinforcement learning algorithms still lack trust in practice due to the risk that the learned policy performs worse than the original policy that generated the dataset or behaves in an unexpected way that is unfamiliar to the user. At the same time, offline RL algorithms are not able to tune their most important hyperparameter - the proximity of the learned policy to the original policy. We propose an algorithm that allows the user to tune this hyperparameter at runtime, thereby overcoming both of the above mentioned issues simultaneously. This allows users to start with the original behavior and grant successively greater deviation, as well as stopping at any time when the policy deteriorates or the behavior is too far from the familiar one.

We call our derived algorithm LION: Learning in Interactive Offline eNvironments.

![Results](/imgs/ib_all_newlabel.png){:class="img-responsive"}
Comparison with prior model-free and model-based offline RL algorithms. We show evaluation performance and distance to the original policy for the LION approach over the chosen λ hyperparameter. State of the art baselines are added as dashed lines with their standard set of hyperparameters (results from [Swazinna et al. 2022](https://arxiv.org/abs/2201.05433)). Even though the baselines all exhibit some hyperparameter that controls the distance to the original policy, all are implemented differently and we can neither map them to a corresponding lambda value of our algorithm, nor change the behavior at runtime, which is why we display them as dashed lines over the entire λ spectrum.