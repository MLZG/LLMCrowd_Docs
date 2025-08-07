# Action

## Definition
An **action** is defined as a behavior or activity that a character can perform within the environment. By executing a specific action, a character can carry out a series of semantically meaningful behaviors. The continuous execution of multiple actions allows a character to engage in sustained, long-term activity within the environment.

## Action Hierarchy
When considering an **action** of a character within the environment, there are many levels of granularity to take into account. For example, a character could be "shopping in a clothing store", "asking the store clerk for advice while shopping", "speaking when asking for advice", or even "slightly raising the arm while gesturing" during the process. All of these are actions performed by the character. However, it is impractical to break down actions to an infinite level of detail. Therefore, in this system, we have designed a hierarchical framework for actions, defining them in a three-tiered hierarchy: **high-level action**, **mid-level action**, and **low-level action**.

### High-level Action
A high-level action is defined as a general decision made by a character within the environment. It can be understood simply as "what should I do next in this shopping mall?" It encapsulates a character’s decision-making process, such as "have meal at restaurant," "browse store," "get a haircut at barber shop," or navigational behaviors, such as "go to the gym". Additionally, high-level actions can involve temporary decisions, such as "wait outside the restaurant" when it is crowded. 

In summary, a high-level action refers to a character’s overarching decision or goal. It typically represents a complete event and is usually a purposeful action with a clear objective.

### Mid-level Action
A mid-level action can be viewed as a breakdown of a high-level action into more specific tasks. Each high-level action corresponds to two types of mid-level actions: **fixed mid-level actions** and **optional mid-level actions**.

  - **Fixed Mid-level Actions**: These are the mandatory sub-actions that must be included when executing a high-level action. For example, the high-level action "have meal" corresponds to the fixed mid-level actions of "order, eat, checkout." Every high-level action is always associated with one or more fixed mid-level actions.

  - **Optional Mid-level Actions**: These represent actions that can be randomly inserted into the execution of the fixed mid-level action sequence, such as "talk with the staff" or "make a phone call." These actions can be seen as random events designed to introduce variety and unpredictability into the execution of high-level actions. A high-level action may have zero or multiple optional mid-level actions.

During the execution of a high-level action, it is typically broken down into several mid-level actions. Note that while most mid-level actions are executed in sequence, some mid-level actions can occur simultaneously, such as "walking" while "making a phone call."

### Low-level Action
A low-level action represents the specific implementation of each mid-level action within the 3D simulation. It includes the character's physical movements, individual actions, interactive actions, cross-actions, and functional tasks. The specific implementation of a low-level action may be closely related to the corresponding mid-level and high-level actions. For further details, please refer to the [Simulator](https://llmcrowd.readthedocs.io/en/latest/developing/simulator/index.html) part.

Generally, **high-level actions** provide an overview of the behavior and broad parameters, while **mid-level actions** further specify the behavior and its parameters. **Low-level actions**, on the other hand, are directly related to the simulator.

## Action Pool

In this system, we have pre-defined a series of plausible action templates, which are organized into an **Action Pool**. This pool represents the set of behaviors available for a character to choose from within the shopping mall environment. 

Both **high-level action** and **mid-level action** are included in the action pool, which are pre-defined with detailed information, including name, description, category, parameters, parameter explanations, and more. Additionally, the action pool also includes the potential structure of the actions.

### Content
The action pool contains a total of 12 high-level actions and 22 mid-level actions.

High-level Action Template:
```json
{
      "action id": <str>,
      "action name": <str>,
      "action category": <str>,
      "action description": <str>,
      "fixed mid-level actions":  <list of str>,
      "optional mid-level actions":  <list of str>,
      "definition of param 1": <str>,
      "definition of param 2": <str>,
      ...
}
```
In the high-level action template, the following fields are pre-defined:
- action id: <str> – The index of the action.
- action name: <str> – The name of the action.
- action category: <str> in ["in_state", "out_state", "transition"] – Specifies whether the action occurs within a state, outside a state, or during a state transition.
- action description: <str> – A natural language description of the action.
- fixed mid-level actions: <list of str> – A sequence of fixed mid-level actions corresponding to the high-level action.
- optional mid-level actions: <list of str> – A list of optional mid-level actions supported by the high-level action.
- definition of param: <str> – Describes the parameters required for the execution of the action, in natural language.

Mid-level Action Template:
```json
{
      "action id": <str>,
      "action name": <str>,
      "action category": <str>,
      "action description": <str>,
      "definition of param 1": <str>,
      "definition of param 2": <str>,
      ...
}
```
In the mid-level action template, the following fields are pre-defined:
- action id: <str> – The index of the action.
- action name: <str> – The name of the action.
- action category: <str> in [fixed, optional, fixed or optional] – Specifies whether the mid-level action is fixed, optional, or can be either.
- action description: <str> – A natural language description of the action.
- definition of param: <str> – Describes the parameters required for the execution of the action, in natural language.

There may be overlap between the parameters of high-level and low-level actions. In such cases, the high-level parameters are more generalized, while the low-level parameters are more specific. If the low-level parameters are sufficient for the simulator, they take precedence. If not, the high-level parameters are used to supplement them.


### Extensibility
The **Action Pool** is built as an extensible structure, supporting the addition of new actions. When expanding the pool, the following aspects usually need to be maintained:
- Add both mid-level actions and high-level actions to the pool. 
- Each state needs to be updated with its corresponding "supported actions."
- The actual implementation of the new action within the simulator must be handled.

In summary, the action pool serves as an expandable library of action templates, covering all possible actions a character can perform in the environment. During pipeline execution, when a specific action is needed, steps like selecting the action template, defining its structure, and setting the parameters should be performed.


## Action Instance
A complete **Action Instance** is defined as a specific high-level action along with its associated sub-structure and parameters. It is instantiated from the action templates within the **Action Pool**.

### Structure:
The structure of an action instance is organized as an ordered multi-way tree, consisting of a high-level action and several mid-level actions as nodes. The root node of the tree represents the high-level action, while the other nodes represent mid-level actions.

In this ordered multi-way tree, except for the root node, nodes at the same level represent actions that are executed sequentially. The parent-child relationship between two nodes (other than the root node) indicates that the actions represented by these nodes are executed concurrently. Here is an example:

<p align = "center">
<img src="../../_static/action_structure/action_instance.jpg" width=80% style="margin-right: 0px; margin-left: 0px;">
</p>

In this example, **dark green** node represents the high-level action, **light green** nodes represent the fixed mid-level actions, and **light orange** nodes represent the optional mid-level actions. The behavior represented by this action instance is as follows: The character first listens to the stuff explain the menu. Then, the character places an order, taking a moment to think about what to order during the process. After placing the order, the character looks around to find a seat. Once seated, the character eats the meal, during which they make a phone call and play with their phone for a while. Finally, the character checkouts and leaves.

As defined, fixed mid-level actions must always be executed and will consistently appear at the second level of the tree, preserving their order. Optional mid-level actions, depending on their timing, can either be placed at the same level, occurring sequentially, or inserted as child nodes to represent concurrent actions.

### Parameters:

Each action instance must include the specific parameter settings for all actions, in accordance with the parameter descriptions defined in the corresponding action templates.


