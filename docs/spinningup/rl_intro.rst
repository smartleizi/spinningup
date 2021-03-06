==========================
第一部分：强化学习中的核心概念
==========================


.. contents:: 目录
    :depth: 2

欢迎来到强化学习的介绍！我们希望你能了解以下内容：

* 常见的符号表示
* 高层次的讲解：关于强化学习算法究竟能做什么（我们会尽量避免 *如何做* 这个话题）
* 算法背后的核心数学知识

总的来说，强化学习是关于智能体以及它们如何通过反复试错来学习的研究。它定义了通过奖励或者惩罚智能体的动作，从而使它未来更容易重复或者放弃某一动作的思想。


强化学习能做什么
===============

基于强化学习的方法已经在很多地方取得了成功。例如，它被用来教电脑在仿真环境下控制机器人：


.. raw:: html

    <video autoplay="" src="https://storage.googleapis.com/joschu-public/knocked-over-stand-up.mp4" loop="" controls="" style="display: block; margin-left: auto; margin-right: auto; margin-bottom:1.5em; width: 100%; max-width: 720px; max-height: 80vh;">
    </video>

以及在现实世界中：

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="https://www.youtube.com/embed/jwSbzNHGflM?ecver=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>
    <br />


他也因为在复杂策略游戏中做出突破而名声大噪，最著名的要数 `围棋`_ 、 `Dota`_ 、教电脑 `玩Atari游戏`_ 以及训练模拟机器人 `听从人类的指令`_ 。

.. _`围棋`: https://deepmind.com/research/alphago/
.. _`Dota`: https://blog.openai.com/openai-five/
.. _`玩Atari游戏`: https://deepmind.com/research/dqn/
.. _`听从人类的指令`: https://blog.openai.com/deep-reinforcement-learning-from-human-preferences/


核心概念和术语
============================

.. figure:: ../images/rl_diagram_transparent_bg.png
    :align: center
    
    智能体和环境的循环作用

强化学习的主要角色是 **智能体** 和 **环境** ,环境是智能体存在和互动的世界。智能体在每一步的交互中，都会获得（有可能只是部分）这个环境的状态的一部分观察，然后决定下一步要采取的动作。环境会因为智能体对它的动作而改变，也可能自己改变。

智能体也会从环境中感知到 **奖励** 信号，一个表明当前状态好坏的数字。智能体的目标是最大化累计奖励，也就是 **回报** 。强化学习就是智能体通过学习来完成目标的方法。

这里我们介绍一些术语，以便后面了解强化学习能做什么：

* 状态和观察
* 动作空间
* 策略
* 行动轨迹
* 不同形式的奖励
* 强化学习优化问题
* 值函数

状态和观察
-----------------------
一个 **状态** :math:`s` 是一个关于这个世界的完整描述，没有隐藏的信息。而 **观察** :math:`o` 是指对于一个状态的部分描述，会漏掉一些信息。

在深度强化学习中，我们会用 `实数向量、矩阵或者更高阶的张量（tensor）`_ 表示状态和观察。比如说，视觉上的观察可以用RGB矩阵的方式表示其像素值；机器人的状态可以通过关节角度和速度来表示。

如果智能体能够对于环境的所有状态有全面的观察，我们通常说环境是被 **全面观察** 的。如果智能体只能观察到一部分，我们称之为 **部分观察**。

.. admonition:: 你应该知道

    强化学习有时候用这个符号 :math:`s` 代表状态 , 有些地方会用观察的符号来表示 :math:`o`.  尤其这种情况会出现：当智能体在决定采取什么动作的时候：我们会用符号表示动作是基于状态的，实际操作的事情，动作是基于观察的，因为智能体并不能知道所有的状态。

    在我们的教程中，我们会严格遵守标准规定，但是你应该能从上下文中看出是什么意思。如果有些内容不清楚，那么，请提issue！我们的目的是教学，不是混淆。

.. _`实数向量、矩阵或者更高阶的张量（tensor）`: https://en.wikipedia.org/wiki/Real_coordinate_space

动作空间
-------------

不同的环境有不同的动作。所有有效动作的集合称之为 **动作空间**。有些环境，比如说 Atari 游戏和围棋，具有的是 **离散动作空间**，这种情况下智能体只能采取有限的动作。其他的一些环境，比如智能体在物理世界控制机器人，属于 **连续动作空间**。在连续动作空间中，动作是实数向量。

这种区别对于深度强化学习有重要的影响。有些种类的算法只能用在某一些案例上，如果想用在另一个案例上可能需要大量的代码重写。

策略
--------

**策略** 是智能体使用的用于决定下一步采取什么行动的规则。可以是确定性的，一般表示为：:math:`\mu`:

.. math::

    a_t = \mu(s_t),

也可以是随机的，一般表示为 :math:`\pi`:

.. math::

    a_t \sim \pi(\cdot | s_t).

因为策略就是智能体的大脑，所以很多时候“策略”和“智能体”经常互换，例如：“策略是要最大化奖励”。

在深度强化学习中，我们处理的是参数化的策略，这些策略的输出是依赖一组参数的计算函数（例如神经网络的权重和误差），我们可以通过策略算法改变智能体的的行为。

我们经常把这些策略的参数写作 :math:`\theta` 或者 :math:`\phi` ，然后把它写作策略的下标来强调两者的联系。

.. math::

    a_t &= \mu_{\theta}(s_t) \\
    a_t &\sim \pi_{\theta}(\cdot | s_t).


确定性策略
^^^^^^^^^^^^^^^^^^^^^^

**例子：确定性策略：** 这是一个简单的基于TensorFlow的在连续动作空间上的确定性的策略：

.. code-block:: python

    obs = tf.placeholder(shape=(None, obs_dim), dtype=tf.float32)
    net = mlp(obs, hidden_dims=(64,64), activation=tf.tanh)
    actions = tf.layers.dense(net, units=act_dim, activation=None)

其中， ``mlp`` 是一个给定大小和激活函数，把多个 ``密集层`` （dense layer）相互堆积在一起的函数

随机性策略
^^^^^^^^^^^^^^^^^^^

The two most common kinds of stochastic policies in deep RL are **categorical policies** and **diagonal Gaussian policies**. 

`Categorical`_ policies can be used in discrete action spaces, while diagonal `Gaussian`_ policies are used in continuous action spaces. 

Two key computations are centrally important for using and training stochastic policies:

* sampling actions from the policy,
* and computing log likelihoods of particular actions, :math:`\log \pi_{\theta}(a|s)`.

In what follows, we'll describe how to do these for both categorical and diagonal Gaussian policies. 

.. admonition:: Categorical Policies

    A categorical policy is like a classifier over discrete actions. You build the neural network for a categorical policy the same way you would for a classifier: the input is the observation, followed by some number of layers (possibly convolutional or densely-connected, depending on the kind of input), and then you have one final linear layer that gives you logits for each action, followed by a `softmax`_ to convert the logits into probabilities. 

    **Sampling.** Given the probabilities for each action, frameworks like Tensorflow have built-in tools for sampling. For example, see the `tf.distributions.Categorical`_ documentation, or `tf.multinomial`_.

    **Log-Likelihood.** Denote the last layer of probabilities as :math:`P_{\theta}(s)`. It is a vector with however many entries as there are actions, so we can treat the actions as indices for the vector. The log likelihood for an action :math:`a` can then be obtained by indexing into the vector:

    .. math::

        \log \pi_{\theta}(a|s) = \log \left[P_{\theta}(s)\right]_a.


.. admonition:: Diagonal Gaussian Policies

    A multivariate Gaussian distribution (or multivariate normal distribution, if you prefer) is described by a mean vector, :math:`\mu`, and a covariance matrix, :math:`\Sigma`. A diagonal Gaussian distribution is a special case where the covariance matrix only has entries on the diagonal. As a result, we can represent it by a vector.

    A diagonal Gaussian policy always has a neural network that maps from observations to mean actions, :math:`\mu_{\theta}(s)`. There are two different ways that the covariance matrix is typically represented.

    **The first way:** There is a single vector of log standard deviations, :math:`\log \sigma`, which is **not** a function of state: the :math:`\log \sigma` are standalone parameters. (You Should Know: our implementations of VPG, TRPO, and PPO do it this way.)

    **The second way:** There is a neural network that maps from states to log standard deviations, :math:`\log \sigma_{\theta}(s)`. It may optionally share some layers with the mean network.

    Note that in both cases we output log standard deviations instead of standard deviations directly. This is because log stds are free to take on any values in :math:`(-\infty, \infty)`, while stds must be nonnegative. It's easier to train parameters if you don't have to enforce those kinds of constraints. The standard deviations can be obtained immediately from the log standard deviations by exponentiating them, so we do not lose anything by representing them this way.

    **Sampling.** Given the mean action :math:`\mu_{\theta}(s)` and standard deviation :math:`\sigma_{\theta}(s)`, and a vector :math:`z` of noise from a spherical Gaussian (:math:`z \sim \mathcal{N}(0, I)`), an action sample can be computed with

    .. math::

        a = \mu_{\theta}(s) + \sigma_{\theta}(s) \odot z,

    where :math:`\odot` denotes the elementwise product of two vectors. Standard frameworks have built-in ways to compute the noise vectors, such as `tf.random_normal`_. Alternatively, you can just provide the mean and standard deviation directly to a `tf.distributions.Normal`_ object and use that to sample.

    **Log-Likelihood.** The log-likelihood of a :math:`k` -dimensional action :math:`a`, for a diagonal Gaussian with mean :math:`\mu = \mu_{\theta}(s)` and standard deviation :math:`\sigma = \sigma_{\theta}(s)`, is given by

    .. math::

        \log \pi_{\theta}(a|s) = -\frac{1}{2}\left(\sum_{i=1}^k \left(\frac{(a_i - \mu_i)^2}{\sigma_i^2} + 2 \log \sigma_i \right) + k \log 2\pi \right).



.. _`Categorical`: https://en.wikipedia.org/wiki/Categorical_distribution
.. _`Gaussian`: https://en.wikipedia.org/wiki/Multivariate_normal_distribution
.. _`softmax`: https://developers.google.com/machine-learning/crash-course/multi-class-neural-networks/softmax
.. _`tf.distributions.Categorical`: https://www.tensorflow.org/api_docs/python/tf/distributions/Categorical
.. _`tf.multinomial`: https://www.tensorflow.org/api_docs/python/tf/multinomial
.. _`tf.random_normal`: https://www.tensorflow.org/api_docs/python/tf/random_normal
.. _`tf.distributions.Normal`: https://www.tensorflow.org/api_docs/python/tf/distributions/Normal

轨迹
------------

A trajectory :math:`\tau` is a sequence of states and actions in the world,

.. math::

    \tau = (s_0, a_0, s_1, a_1, ...).

The very first state of the world, :math:`s_0`, is randomly sampled from the **start-state distribution**, sometimes denoted by :math:`\rho_0`:

.. math::

    s_0 \sim \rho_0(\cdot).

State transitions (what happens to the world between the state at time :math:`t`, :math:`s_t`, and the state at :math:`t+1`, :math:`s_{t+1}`), are governed by the natural laws of the environment, and depend on only the most recent action, :math:`a_t`. They can be either deterministic,

.. math::

    s_{t+1} = f(s_t, a_t)

or stochastic,

.. math::

    s_{t+1} \sim P(\cdot|s_t, a_t).

Actions come from an agent according to its policy.

.. admonition:: You Should Know

    Trajectories are also frequently called **episodes** or **rollouts**.


奖励和返回
-----------------

The reward function :math:`R` is critically important in reinforcement learning. It depends on the current state of the world, the action just taken, and the next state of the world:

.. math::

    r_t = R(s_t, a_t, s_{t+1})

although frequently this is simplified to just a dependence on the current state, :math:`r_t = R(s_t)`, or state-action pair :math:`r_t = R(s_t,a_t)`. 

The goal of the agent is to maximize some notion of cumulative reward over a trajectory, but this actually can mean a few things. We'll notate all of these cases with :math:`R(\tau)`, and it will either be clear from context which case we mean, or it won't matter (because the same equations will apply to all cases).

One kind of return is the **finite-horizon undiscounted return**, which is just the sum of rewards obtained in a fixed window of steps:

.. math::

    R(\tau) = \sum_{t=0}^T r_t.

Another kind of return is the **infinite-horizon discounted return**, which is the sum of all rewards *ever* obtained by the agent, but discounted by how far off in the future they're obtained. This formulation of reward includes a discount factor :math:`\gamma \in (0,1)`:

.. math::

    R(\tau) = \sum_{t=0}^{\infty} \gamma^t r_t.


Why would we ever want a discount factor, though? Don't we just want to get *all* rewards? We do, but the discount factor is both intuitively appealing and mathematically convenient. On an intuitive level: cash now is better than cash later. Mathematically: an infinite-horizon sum of rewards `may not converge`_ to a finite value, and is hard to deal with in equations. But with a discount factor and under reasonable conditions, the infinite sum converges.

.. admonition:: You Should Know

    While the line between these two formulations of return are quite stark in RL formalism, deep RL practice tends to blur the line a fair bit---for instance, we frequently set up algorithms to optimize the undiscounted return, but use discount factors in estimating **value functions**. 

.. _`may not converge`: https://en.wikipedia.org/wiki/Convergent_series

强化学习问题
--------------


Whatever the choice of return measure (whether infinite-horizon discounted, or finite-horizon undiscounted), and whatever the choice of policy, the goal in RL is to select a policy which maximizes **expected return** when the agent acts according to it.

To talk about expected return, we first have to talk about probability distributions over trajectories. 

Let's suppose that both the environment transitions and the policy are stochastic. In this case, the probability of a :math:`T` -step trajectory is:

.. math::

    P(\tau|\pi) = \rho_0 (s_0) \prod_{t=0}^{T-1} P(s_{t+1} | s_t, a_t) \pi(a_t | s_t).


The expected return (for whichever measure), denoted by :math:`J(\pi)`, is then:

.. math::

    J(\pi) = \int_{\tau} P(\tau|\pi) R(\tau) = \underE{\tau\sim \pi}{R(\tau)}.


The central optimization problem in RL can then be expressed by

.. math::

    \pi^* = \arg \max_{\pi} J(\pi),

with :math:`\pi^*` being the **optimal policy**. 


值函数
---------------

It's often useful to know the **value** of a state, or state-action pair. By value, we mean the expected return if you start in that state or state-action pair, and then act according to a particular policy forever after. **Value functions** are used, one way or another, in almost every RL algorithm.


There are four main functions of note here.

1. The **On-Policy Value Function**, :math:`V^{\pi}(s)`, which gives the expected return if you start in state :math:`s` and always act according to policy :math:`\pi`:

    .. math::
        
        V^{\pi}(s) = \underE{\tau \sim \pi}{R(\tau)\left| s_0 = s\right.}

2. The **On-Policy Action-Value Function**, :math:`Q^{\pi}(s,a)`, which gives the expected return if you start in state :math:`s`, take an arbitrary action :math:`a` (which may not have come from the policy), and then forever after act according to policy :math:`\pi`:

    .. math::
        
        Q^{\pi}(s,a) = \underE{\tau \sim \pi}{R(\tau)\left| s_0 = s, a_0 = a\right.}


3. The **Optimal Value Function**, :math:`V^*(s)`, which gives the expected return if you start in state :math:`s` and always act according to the *optimal* policy in the environment:

    .. math::

        V^*(s) = \max_{\pi} \underE{\tau \sim \pi}{R(\tau)\left| s_0 = s\right.}

4. The **Optimal Action-Value Function**, :math:`Q^*(s,a)`, which gives the expected return if you start in state :math:`s`, take an arbitrary action :math:`a`, and then forever after act according to the *optimal* policy in the environment:

    .. math::

        Q^*(s,a) = \max_{\pi} \underE{\tau \sim \pi}{R(\tau)\left| s_0 = s, a_0 = a\right.}


.. admonition:: You Should Know

    When we talk about value functions, if we do not make reference to time-dependence, we only mean expected **infinite-horizon discounted return**. Value functions for finite-horizon undiscounted return would need to accept time as an argument. Can you think about why? Hint: what happens when time's up?

.. admonition:: You Should Know

    There are two key connections between the value function and the action-value function that come up pretty often:

    .. math::

        V^{\pi}(s) = \underE{a\sim \pi}{Q^{\pi}(s,a)},

    and

    .. math::

        V^*(s) = \max_a Q^* (s,a).

    These relations follow pretty directly from the definitions just given: can you prove them?

The Optimal Q-Function and the Optimal Action
---------------------------------------------

There is an important connection between the optimal action-value function :math:`Q^*(s,a)` and the action selected by the optimal policy. By definition, :math:`Q^*(s,a)` gives the expected return for starting in state :math:`s`, taking (arbitrary) action :math:`a`, and then acting according to the optimal policy forever after. 

The optimal policy in :math:`s` will select whichever action maximizes the expected return from starting in :math:`s`. As a result, if we have :math:`Q^*`, we can directly obtain the optimal action, :math:`a^*(s)`, via

.. math::

    a^*(s) = \arg \max_a Q^* (s,a).

Note: there may be multiple actions which maximize :math:`Q^*(s,a)`, in which case, all of them are optimal, and the optimal policy may randomly select any of them. But there is always an optimal policy which deterministically selects an action.


贝尔曼方程
-----------------

All four of the value functions obey special self-consistency equations called **Bellman equations**. The basic idea behind the Bellman equations is this:

    The value of your starting point is the reward you expect to get from being there, plus the value of wherever you land next.


The Bellman equations for the on-policy value functions are

.. math::
    :nowrap:

    \begin{align*}
    V^{\pi}(s) &= \underE{a \sim \pi \\ s'\sim P}{r(s,a) + \gamma V^{\pi}(s')}, \\
    Q^{\pi}(s,a) &= \underE{s'\sim P}{r(s,a) + \gamma \underE{a'\sim \pi}{Q^{\pi}(s',a')}},
    \end{align*}

where :math:`s' \sim P` is shorthand for :math:`s' \sim P(\cdot |s,a)`, indicating that the next state :math:`s'` is sampled from the environment's transition rules; :math:`a \sim \pi` is shorthand for :math:`a \sim \pi(\cdot|s)`; and :math:`a' \sim \pi` is shorthand for :math:`a' \sim \pi(\cdot|s')`. 

The Bellman equations for the optimal value functions are

.. math::
    :nowrap:

    \begin{align*}
    V^*(s) &= \max_a \underE{s'\sim P}{r(s,a) + \gamma V^*(s')}, \\
    Q^*(s,a) &= \underE{s'\sim P}{r(s,a) + \gamma \max_{a'} Q^*(s',a')}.
    \end{align*}

The crucial difference between the Bellman equations for the on-policy value functions and the optimal value functions, is the absence or presence of the :math:`\max` over actions. Its inclusion reflects the fact that whenever the agent gets to choose its action, in order to act optimally, it has to pick whichever action leads to the highest value.

.. admonition:: You Should Know

    The term "Bellman backup" comes up quite frequently in the RL literature. The Bellman backup for a state, or state-action pair, is the right-hand side of the Bellman equation: the reward-plus-next-value. 


Advantage Functions
-------------------

Sometimes in RL, we don't need to describe how good an action is in an absolute sense, but only how much better it is than others on average. That is to say, we want to know the relative **advantage** of that action. We make this concept precise with the **advantage function.**

The advantage function :math:`A^{\pi}(s,a)` corresponding to a policy :math:`\pi` describes how much better it is to take a specific action :math:`a` in state :math:`s`, over randomly selecting an action according to :math:`\pi(\cdot|s)`, assuming you act according to :math:`\pi` forever after. Mathematically, the advantage function is defined by

.. math::

    A^{\pi}(s,a) = Q^{\pi}(s,a) - V^{\pi}(s).

.. admonition:: You Should Know

    We'll discuss this more later, but the advantage function is crucially important to policy gradient methods.



（可选）数学模型
====================

我们已经非正式地讨论了智能体的环境，但是如果你深入研究，可能会发现这样的标准数学形式：**马尔科夫决策过程** (Markov Decision Processes, MDPs).MDP是一个5元组 :math:`\langle S, A, R, P, \rho_0 \rangle`, 其中


* :math:`S` 是所有有效状态的集合,
* :math:`A` 是所有有效动作的集合,
* :math:`R : S \times A \times S \to \mathbb{R}` 是奖励函数，其中 :math:`r_t = R(s_t, a_t, s_{t+1})`,
* :math:`P : S \times A \to \mathcal{P}(S)` 是转态转移的规则，其中 :math:`P(s'|s,a)` 是在状态  :math:`s` 下 采取动作 :math:`a` 转移到状态 :math:`s'` 的概率。 
* and :math:`\rho_0` is the starting state distribution.





.. _`Markov property`: https://en.wikipedia.org/wiki/Markov_property

