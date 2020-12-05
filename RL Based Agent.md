# RL Based Agent

## 运行方式

## 方法选择

基于打表的q_learning

训练中用时序差分TD来更新q_table：

```python
q_table[prev_state][action] = 0.5 * q_table[prev_state][action] + 0.5 * (reward + 0.99 * max_q_next)
```

训练和测试时的动作选择都采用epsilon-greedy.

```python
epsilon -> action = np.argmax(q_table[curr_state][:]) 
1-epsilon -> action = np.random.randint(0, num_action)
```



## FindAndDefeatZerglings

##### states x actions = 4 x 2

state_1：视野内敌人的数量，数据分箱[-inf, 1, 2, 3, inf]

action_1：攻击最近的敌人

action_2：沿给定路线巡逻

##### reward：

直接用pysc2库给出的obs.reward，获取每一步的即时收益





### 效果对比

|                        | Mean, Max (Rule-based) | Mean, Max (Rule-based) | Mean, Max (Worst in paper) | Mean, Max (Best in paper) |
| ---------------------- | ---------------------- | ---------------------- | :------------------------- | ------------------------- |
| FindAndDefeatZerglings | 46.4，54               | 46.68, 52              | 45, 56                     | 49, 59                    |

<img src="D:\2020Autumn\RL\rl-midterm-master\test_100ep.png" style="zoom:150%;" />

episode：100

### 效果分析

将视野内敌人的数量作为状态，Rule-Based认为只要有敌人就都选择attack，没有敌人就scout，相当于它的策略如下：

|         | scout | attack |
| ------- | ----- | ------ |
| 0 zerg  | √     |        |
| 1 zerg  |       | √      |
| 2 zerg  |       | √      |
| ≥3 zerg |       | √      |

RL-Based学出的是类似的效果，q_table可以反映出来策略偏好：

| 15.55 | 18.69 |
| ----- | ----- |
| 20.44 | 19.22 |
| 21.27 | 18.72 |
| 21.39 | 20.30 |

只是注意到过分偏离路线的Attack会导致遍历地图的速度变慢，所以测试时选择epsilon-greedy而非greedy，保留了在视野里有敌人的情况下仍然前行的可能性，这是RL_Based在Max_score上比Rule_Based表现较好的原因。









