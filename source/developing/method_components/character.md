# Character

<!--
【character】
我有一大堆characters, 每个都有一系列attributes
character attributes


关于state的转换。
一个角色会维护它的动态属性，来表示这个角色目前所处的state。通常，这个属性由(state, status)来表示，比如(cloth shop, in)，(gym, out)，分别表示角色在cloth shop中，和在gym外(门口)。(None, out)表示角色目前不处于任何一个state。state表示目前角色所处的state，status表示角色是在这个state中还是在state外。
这个属性和actions存在关联。当角色处于某个state，并执行该state所support的"in_state"类型action时，角色的status就会转变成in。当角色执行"transition"类型的action时，角色的state会变为新的state，而status也会变成out。也就是说，"in_state"类型的action通常隐含包括了进入该state(如果本身就在该state内就不会再次进入)。而"transition"类型的action隐含包括了离开当前state，并前往某个state外。
需要注意的是，角色执行action时往往需要满足约束条件，角色只有在当前state和要执行的in_state action的state_id相同时，才能够执行。
其实就相当于是给每个in_state action前面前缀了一个"enter"，给每个transition action前面前缀了一个"exit"。但是这里我们忽略了enter和exit这两个action，因为角色有可能在同一个state中连续执行两个high-level action，总不能enter两次。也有可能在out状态下连续执行两次go to，比如先去了衣服店，但是需要wait，wait一会就走了，又去了健身房，也不能exit两次。
entry|in, go to cloth shop
cloth shop|out, consult cloth shop
cloth shop|in, browse cloth shop
cloth shop|in, go to gym
gym|out, exercise gym
gym|in, go to exit
exit|out, -
-->