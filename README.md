# deep_reinforcement_learning
1.强化学习中有四个关键量:<X,A,R,P>
      X:状态空间
      A：动作空间
      R：奖励
      P：状态转移函数

2.实验中能得到的数据：
    在某个状态x_t下，执行动作a， 状态转移到x_t+1，并获取该动作奖励r_t

3.强化学习的目标：
    强化学习方法是一种顾全局的学习方法，即在状态x_t下执行动作a之后，能使r_t加上之后产生的累积奖励最大，而不是只图当前r_t最大。
    因此我们需要找到一个能描述在状态x_t下执行动作a_t之后产生的累计奖励函数，即Q(x,a),注意Q（x,a）包括当前动作a_t产生的奖励r_t,而Q(x，a)往往是一个关于矩阵的函数，而输出则是一个向量，向量的维数与动作空间维数一样，即每一个元素表示执行相应动作会产生多大的累计奖励，强化学习的目标就是为了找到这样的一个Q（x，a），当你在之后应用中，输入一个状态x,它能产生一个列向量，然后你再输入一个动作（即你要取这个列向量的第几个元素，因为一个元素对应一个动作产生的奖励），就可以获得Q(x,a),然后选择能使得Q（x,a）最大的那个动作。

4.如何选取Q（x，a） 的形式：
    只要解得Q（x,a），那么强化学习的问题就得到了解决。但我们首先需要为Q（x,a）假设一个合理的形式，以DQN算法为例，假设Q（x,a)是一个CNN。还可以假设成别的形式，比如svm等等。

5.Q(x,a)的迭代算法：
    假设了Q(x,a)的形式之后还需要一个迭代格式，使得随着数据不断增多时，Q(x,a)能够收敛，采用的迭代策略是，我们随机初始化Q(x,a)中的待求解变量，输入一个x_t，与动作a_t，他能获得基于当前Q（x,a）的累计奖励最大的输出记为Q（x_t,a_t）_old，同时，我们通过做实验(模拟器)输入x_t，执行同样的动作a_t,可以获得奖励r_t以及下一个状态x_t+1。Q（x_t,a_t）除了可以用Q(x,a)直接计算出来之外，还可以通过实验实际奖励和递推公式获得即：
                                                Q(x_t,a_t)=r_t+gamma*max（Q(x_t+1,a_t+1)）
    我们把用这种递推公式加上实验奖励计算得到的Q（x_t,a_t）记为Q（x_t,a_t）_new,为什么要加上gamma，因为此处用的gamma折扣累计奖励，不是简单的将每步奖励叠加，而是越靠后的奖励最累计奖励的共享越小。
    因此我们使得对每组数据我们使得Q（x_t,a_t）_old-Q（x_t,a_t）_new最小，我们采用的方法是，先大量的用模拟器采样，然后抽取数据不算优化Q（x,a）中的变量，这个过程可以看作是监督学习。

6.Q（x,a）变量的优化方法：
    优化的方法有很多，可以在tensorflow的优化器中选择所需要的优化器，对参数进行优化。程序中使用Adam优化方法