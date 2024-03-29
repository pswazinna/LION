---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---
{: style="text-align:center"}
[Phillip Swazinna](https://scholar.google.de/citations?user=eqDGnSkAAAAJ&hl=en&oi=ao) - [Steffen Udluft](https://scholar.google.de/citations?user=GHLtt4cAAAAJ&hl=en&oi=ao) - [Thomas Runkler](https://scholar.google.de/citations?user=9ulZrB8AAAAJ&hl=en&oi=ao)

{: style="text-align:center"}
[OpenReview](https://openreview.net/forum?id=a4COps0uokg) - [ArXiv](https://arxiv.org/abs/2205.10629) - [Code](https://github.com/siemens/lion)

We present LION: Learning in Interactive Offline eNvironments - an offline RL algorithm that may at runtime change its behavior based on a user's personal preference regarding the proximity to previously observed behavior, resolving issues related to both trust and hyperparameter tuning for offline reinforcement learning.


&nbsp;  

## Industrial Benchmark Results
![Results](/imgs/ib_all_newlabel.png){:class="img-responsive"}

Comparison with prior model-free and model-based offline RL algorithms on the Industrial Benchmark. We show evaluation performance and distance to the original policy for the LION approach over the chosen λ hyperparameter. State of the art baselines are added as dashed lines with their standard set of hyperparameters (results from [Swazinna et al. 2022](https://arxiv.org/abs/2201.05433)). Even though the baselines all exhibit some hyperparameter that controls the distance to the original policy, all are implemented differently and we can neither map them to a corresponding lambda value of our algorithm, nor change the behavior at runtime, which is why we display them as dashed lines over the entire λ spectrum.


&nbsp;  

## Abstract
Offline reinforcement learning algorithms still lack trust in practice due to the risk that the learned policy performs worse than the original policy that generated the dataset or behaves in an unexpected way that is unfamiliar to the user. At the same time, offline RL algorithms are not able to tune their most important hyperparameter - the proximity of the learned policy to the original policy. We propose an algorithm that allows the user to tune this hyperparameter at runtime, thereby overcoming both of the above mentioned issues simultaneously. This allows users to start with the original behavior and grant successively greater deviation, as well as stopping at any time when the policy deteriorates or the behavior is too far from the familiar one.


&nbsp;  

## Algorithm Architecture
![Schematic](/imgs/lion_visual_crop.png){:class="img-responsive"}

Schematic of LION policy training. Only π (in green) is trained, while the original
policy model β (orange) and the dynamics ensemble {f} (blue) are already trained and remain
unchanged. λ is sampled individually for every single imagined trajectory.


&nbsp;  

## 2D World Example
{:refdef: style="text-align: center;"}
![2DWorld](/imgs/basics_simple.png){:width="400"}
{: refdef}

In a simple 2D environment with data collecting policy as shown in (a) and rewards distributed according to a gaussian around a fixed point as in (b), we can easily visualize how the trained policy changes for different λ at test-time: For low values it remains close to the original policy, while for increasing values it more and more disregards the collecting policy and moves to optimize the return.

![2DPolicy](/imgs/combined_simple.png){:class="img-responsive"}


&nbsp;  


## Other Adaptive Baselines
{:refdef: style="text-align: center;"}
![2DWorld](/imgs/rvs.png){:class="img-responsive"}
{: refdef}

An extremely simple method to obtain similar results would be to just train a pre-existing offline RL algorithm for a discrete number of hyperparameter values and then deliver the set of resulting policies to the user, who can then at runtime decide which policy to use. Similarly to LION, the user could then potentially tune the hyperparameter and find a good trade-off between familiarity of the solution and performance. Two downsides are immediately obvious: (i) The approach's compute cost grows linearly with the number of values considered, and (ii) since only a discrete set of policies is provided, the user is limited in their choice. Even worse however, we find that prior offline algorithms like MOPO do not necessarily behave consistently over the range of hyperparameters, as can be seen in the Figure above: The return landscape is a constant zigzag, making it hardly usable in practice.

Additionally, we consider another instance of conditional policies that deliver a continuous solution: Recently, approaches that condition on the return-to-gohave become popular for offline RL. In our experiments however, the RvS baseline in the Figure below does not exhibit sufficiently diverse behavior over the course of the hyperparameters.

![2DPolicy](/imgs/multi_mopo.png){:class="img-responsive"}


&nbsp;  

## Influence of λ-Distribution
{:refdef: style="text-align: center;"}
![Betas](/imgs/betas.png){:class="img-responsive"}{: width="500"}
{: refdef}

We do not sample λ uniformly since doing so leads to less accurate learning at the edges of the λ-spectrum. Above, we show visualizations of different symmetric Beta distributions from which we draw λ together with the policy results on the bad-0.2 dataset. The Beta(1, 1) case corresponds to the uniform distribution and has a significant discrepancy at the λ=0 end of the range: The original policy (its performance) is not accurately reproduced. Generally, it seems the more flat the distribution becomes, the more are the two extreme cases moved together. We observe this phenomenon even though the policy has plenty of capacity.


&nbsp;  

## Model-free Experiments
![Modelfree](/imgs/ib_value_baselines.png){:class="img-responsive"}

While it should be possible to derive a similar algorithm as LION in the model-free world, we were not quite able to do so: The figure above shows an experiment in which we augment TD3+BC with our approach by sampling λ from the same distribution as in LION and providing it to the policy, only that now it controls the influence the value function has on the optimization of the policy and (1-λ) controls the distance to the behavior actions. Interestingly however, the trained policy is unable to alter its behavior anywhere in the λ-range. Instead it seems that the policy training is rather unstable and the policy collapses to a solution that behaves always the same, regardless of λ$. Similarly, the distance to the original policy does not change with varying λ.
