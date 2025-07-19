# Behavioral Trajectory

<!--
【Behavioral trajectory】
一个完整的human行为，定义为一个完整的行为sequence(比如一天内的完整行为)，一个【behavior】代表了一条完整的行为轨迹：[(state, context, action)， (state, context, action), ...]
这也是LLM需要生成的内容，LLM的任务目标就是为每个character在每个time step，做action的选择，以组成一个完整的behavior。
其中，
- state代表character目前所处的环境中的state，只有在执行状态转移类型的action且执行成功时才会改变；
- context代表character在目前所处环境中，可以观测到的上下文，用以影响其当前的决策。包括character的memory，上一步action执行结束之后的feedback,以及对当前state或者surrounding的observation(密度等等)。
  [memory, action_feedback, state_observation]
  这部分可以再想想，其实就是我要告诉LLM什么内容：我以前做过什么事情(memory)，我现在在哪(state)，我刚刚做完了什么事情(action feedback)，我周围有什么，我能感知到什么(observation， PS:observation is related to action feedback)。就是我要告诉LLM的上下文。
  比如我现在再理发店，刚刚到，看到理发店满员了；比如我现在在衣服店，刚browse完出来，(选: 看到附近有XXX)。
- action代表【人物】在目前所处的state下，基于context的内容，做出的action。每个action都是一个structural的high-level action，可以分解为多个mid-level action的sequence。
  action在实际执行结束后会返回一些feedback，比如执行成功与否，执行结束之后的观测，作为下个time step的context的一部分。
Example behavior：
[(entrance, new, go to cloth store),
 (cloth store 1, [new | sparse], browse),
 (cloth store 1, [browsed in cloth store 1 | done-out], go to barbershop),
 (barbershop, [browsed in cloth store 1 | full], wait),
 (barbershop, [browsed in cloth store 1, waited for barbershop | can enter barbershop], haircut),
 (barbershop, [browsed in cloth store 1, waited for barbershop, get haircut | done-out], go to pet store),
 (pet store, [browsed in cloth store 1, waited for barbershop, get haircut | crowded], go to exit),
 (exit, -, -)
]
啊啊啊，每个(state, context, action)还要有对应的时间标签(start time, end time)，均对应真实世界时间。
这个过程中我们是要记录各种各样的信息的。估计dataset component也要详细列一个表格出来。
-->

