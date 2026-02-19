---
title: 'Game Theory for Machine Learning: A Perspective'
date: 2023-08-18
permalink: /posts/2023/08/game-theory-for-machine-learning/
tags:
  - machine learning
  - technical
---

Game theory is a system of mathematical modelling that studies the interactions between self interested parties. That is, players who can make decisions on their own. These can be anything from tic-tac-toe to deciding how to invest in stocks to decisions of war and peace between countries.

<figure>
  <img src="/images/game_theory_splash_image.jpg" alt="Game Theory for Machine Learning" style="width: 100%;">
</figure>

The field took off with a firm background when the celebrated mathematician, John Nash, often famous for his biopic "A Beautiful Mind", proved the idea that a Nash Equilibrium exists for every finite game. This statement can be simplified to, in every game with a finite number of players and possible moves, there exists at least one set of strategies, when played by all the players, results in a standoff where no one player can improve their position by themselves. Classic examples of game theory include the Prisoner's Dilemma, where Nash seems so against intuition because the only Pareto-dominated strategy is the Nash Equilibrium. This means that the only strategy that does not allow for a gain to any player without hurting the others is the only way to ensure that both the players also maintain their current payoffs. Let it be noted that the theorem only speaks of the existence and not of any way of finding the Nash Equilibria of a game. That is a problem spanning an entire complexity class in itself (complexity class PPAD).

Game theory gained relevance in machine learning when methods like Generative Adversarial Networks (GANs), where more than one neural networks optimise their own loss functions to compete or fool the other, were introduced. These models are immensely powerful and have created entirely new avenues to generative modelling, the branch of machine learning concerned with creating instead of classifying. However, they also posed significant new challenges. GANs are notoriously difficult to train and are ridden with all sorts of eccentricities like mode collapse, where the generator just decides to learn one way to fool the discriminator expertly instead of all the types of data we want it to learn. A data set consisting of all animals may have the generator producing only cats, and only brown cats, if you fall into the ditch of mode collapse.

GANs are an attempt to train two neural networks who are in a zero-sum game, which is to say, when one optimises its loss function, it adds to the loss of the other. Simple minded gradient descent and its descendants, that look at just the current state and gradient of the loss function of just one of the players, obviously have trouble optimising in such complicated terrains. They do not take into consideration the fact that a change in the current move may also affect how the other network decides its own next move. Enter game theory, which just so happens to specialise in modelling such situations.

A very interesting series of papers on this topic is by Schaefer et al, who build upon previous work in the field to come up with the competitive gradient descent, which they claim is a natural extension to gradient descent in the multiplayer setting. Formulated for two player situations, this technique takes into consideration not just what each agent does in the optimization process, but also what the other agent might do. This is achieved by introducing the hessian of the loss functions to the optimisation process. The hessian term represents interaction between both the players as the elements of the hessian, being second derivatives of the loss functions, are dependent on the actions of both the players.

<figure>
  <img src="/images/game_theory_competitive_gradient_descent.jpg" alt="Competitive Gradient Descent" style="width: 100%;">
  <figcaption>Schaefer et al, "Competitive gradient descent"</figcaption>
</figure>

Where f and g are the loss functions of the discriminator and generator respectively.

The second term in the equations corresponds to the Nash equilibrium of a bilinear approximation of the gradient descent problem. The paper demonstrates how CGD does well in many experiments without diverging or suffering from mode collapse even in situations where the gradient descent and many of the other proposed methods do.

An interesting follow up work, 'Implicit Competitive Regularisation in GANs' challenges the minimax interpretation which was introduced in the original GAN paper. According to Goodfellow et al, the discriminator learns by trying to minimise its loss via correctly identifying fake data points while the generator tries to maximise the loss of the discriminator by generating samples that are really similar to the distribution we are trying to learn from. This minimax optimisation, according to the authors, is the basis for the success of the GANs.

Schaefer et al note that this cannot be the only reason. They point out the following flaws in this reasoning. If GANs are trained without regularisation in the loss function, as in the original paper, the discriminator network can always overtrain on the dataset forcing the generator to also learn very few specific examples from the dataset, if at all they are able to generate anything. This is particularly bad for generative models. They should be able to produce novel samples from the distribution that they are trying to learn. However we know that such GANs work in producing generalised data samples. Though on the other hand, if regularisation constraints are applied, the method of regularisation is a major challenge in explaining how GANs work so well. A GAN discriminator translates an image from its pixel by pixel form to a score of how realistic it is. Taking the euclidean norm of the gradients of such a function is only a measure of how similar the pixels in the two images are and not of how visually similar the two images look. The same can be said of most other regularisation methods used. However this method works remarkably well and also mitigates some of the instability issues that plague metric agnostic GANs.

This leaves a question of how GANs are able to produce the remarkable quality of images that they do. In the paper, the authors present the notion of an implicit competitive regularisation (ICR), that is a regularisation of the discriminator arising from the optimisation procedure of training two models together and not from any added regularisation terms in the loss functions. It is well known that neural networks gain inductive biases from their structure. Neural networks are universal function approximators and can, theoretically, learn any function between inputs and outputs, but some networks perform better in certain types of data like the Recurrent Neural Networks work better than fully connected systems for sequence models (like sound or wave patterns) because the architecture imbibes it with a bias towards learning such relations from the data. Similarly implicit regularisation is a bias arising from the optimisation procedure. Training the same model under these different procedures may lead to differing results. Some may "prefer" some minima in the data over the other, and thus produce more generalisable results. ICR plays this role in the multi agent setting. The paper claims that ICR converges the networks to points on the landscape that can be considered as Nash Equilibria and produce great quality samples and goes on to show how competitive gradient descent can increase ICR and stabilise GAN training even without explicit regularisation.

Another interesting work in the intersection of game theory and deep learning is "Interpreting and boosting dropout from a game theoretic point of view" by Zhang et al. The work seeks to explain and prove the efficiency in dropout regularisation, a commonly used technique to avoid overfitting in neural networks, by looking at the process as a game. The paper models dropout using Shapley values, a game theoretic concept that won the Nobel prize in 2012. Shapley values are a fair measure of distribution of the utility of a game. It describes the sharing of rewards of the game amongst a group of players in a coalition in accordance with their contribution to securing said reward for their coalition.

In their work, Zhang et al consider a neural network as a game played by the input variables. The interaction strength between two variables is the difference between the Shapley value of the network when both the inputs are supplied and the sum of the Shapley values when either one of the inputs are replaced by a baseline.

<figure>
  <img src="/images/game_theory_interpreting_dropout.png" alt="Interpreting dropout from a game theoretic point of view" style="width: 100%;">
  <figcaption>Zhang et al, "Interpreting and boosting dropout from a game theoretic point of view"</figcaption>
</figure>

If this value is positive for a pair of variables, they cooperate to increase the output while negative interaction shows the output decreases as the variables are adversarial. Interaction strength is the magnitude of the value.

The authors then prove that dropout reduces the interaction in networks and thus overfitting. Based on these insights they design the interaction loss, a sampled approximation of the computationally expensive process of calculating interactions. Interaction loss can be added to the usual classification loss in a neural network to give a stronger control over the interaction than the dropout. Furthermore, interaction loss can be used along with batch normalisation, another very popular regularisation technique, which is known to cause irregular results when used together with dropout.

"Eigengames: PCA as a Nash Equilibrium" by Gemp et al presents another fascinating venue of work. As the paper title suggests, they model Principal Component Analysis as a game played by the eigenvectors of the data covariance matrix. The interactions between the various possible eigenvectors are suppressed in the formulation so that the first player only tries to capture the maximum variance while each of the successive eigen vectors each also have to try to orthogonalise their alignment from their predecessors. This reformulation results in a set of loss functions that are almost independent and can each be computed parallely with some message passing between computing nodes allowing for a scalable approach to compute PCA onto huge datasets. It is also proved that the Nash equilibrium of this particular setup is the principal components of the data.

The authors go on to prove this by attempting to find the top 32 principal vectors on the activations of ResNet-200, almost 200 TB in size, a feat almost impossible for earlier algorithms in about 9 hours using Google's proprietary TPU hardware. Furthermore the algorithm also had provable guarantees on the results irrespective of the initialisation, a fatal flaw to some of the earlier approaches.

<figure>
  <img src="/images/game_theory_eigengames.png" alt="EigenGame: PCA as a Nash Equilibrium" style="width: 100%;">
  <figcaption>Gemp et al, "EigenGame: PCA as a Nash Equilibrium"</figcaption>
</figure>

It is worth noting that the hierarchy imposed on the players is a very important factor in this formulation. It suppresses the amount of interaction between players by many fold, forcing all the successors to react to the changes made by the earlier eigenvectors, simplifying the game to make solutions easier.

These are some of the many many examples of excellent work conducted across the world. From understanding existing mechanisms to improving the very basics, game theory has much to offer to machine learning. As complexity of machine learning algorithms increases, it is important to think beyond using more computational resources and start rethinking the steps in algorithm design. Game theory might also help alleviate some of the mystery behind the 'black box' model of machine learning and help us understand more of the workings of the intricate structure of nodes in neural nets.
