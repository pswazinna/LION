---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---
{: style="text-align:center"}
Phillip Swazinna - Steffen Udluft - Thomas Runkler

{: style="text-align:center"}
[Read the Full Paper](https://arxiv.org/abs/2205.10629)

___


We present LION: Learning in Interactive Offline eNvironments - an offline RL algorithm that may at runtime change its behavior based on a user's personal preference regarding the proximity to previously observed behavior, resolving issues related to both trust and hyperparameter tuning for offline reinforcement learning.


![Results](/imgs/ib_all_newlabel.png){:class="img-responsive"}
Comparison with prior model-free and model-based offline RL algorithms. We show evaluation performance and distance to the original policy for the LION approach over the chosen λ hyperparameter. State of the art baselines are added as dashed lines with their standard set of hyperparameters (results from [Swazinna et al. 2022](https://arxiv.org/abs/2201.05433)). Even though the baselines all exhibit some hyperparameter that controls the distance to the original policy, all are implemented differently and we can neither map them to a corresponding lambda value of our algorithm, nor change the behavior at runtime, which is why we display them as dashed lines over the entire λ spectrum.

___


### Abstract
Offline reinforcement learning algorithms still lack trust in practice due to the risk that the learned policy performs worse than the original policy that generated the dataset or behaves in an unexpected way that is unfamiliar to the user. At the same time, offline RL algorithms are not able to tune their most important hyperparameter - the proximity of the learned policy to the original policy. We propose an algorithm that allows the user to tune this hyperparameter at runtime, thereby overcoming both of the above mentioned issues simultaneously. This allows users to start with the original behavior and grant successively greater deviation, as well as stopping at any time when the policy deteriorates or the behavior is too far from the familiar one.


___


### Algorithm Architecture
![Schematic](/imgs/lion_visual_crop.png){:class="img-responsive"}
Schematic of LION policy training. Only π (in green) is trained, while the original
policy model β (orange) and the dynamics ensemble {f} (blue) are already trained and remain
unchanged. λ is sampled individually for every single imagined trajectory.


___


### 2D World Example
![2DWorld](/imgs/basics_simple.png){:class="img-responsive"}
In a simple 2D environment with data collecting policy as shown in (a) and rewards distributed according to a gaussian around a fixed point as in (b), we can easily visualize how the trained policy changes for different λ at test-time: For low values it remains close to the original policy, while for increasing values it more and more disregards the collecting policy and moves to optimize the return.
![2DPolicy](/imgs/combined_simple.png){:class="img-responsive"}