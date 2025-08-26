# Character

## Definition
A *character* is defined as a pedestrian within the environment that can move and perform actions. Each character is an independent individual capable of carrying out a variety of basic and interactive actions, such as walking, running, waving, browsing products, conversing with others, and more (see the [Action](https://llmcrowd.readthedocs.io/en/latest/developing/method_components/action.html) section for further details).

Each character is assigned a set of unique attributes to differentiate it, such as id, gender, age, personality, education, and others. Characters can act alone or form groups with other characters. Additionally, each character may have *relationships* with other characters, which are further explained in the [Interaction](https://llmcrowd.readthedocs.io/en/latest/developing/method_components/interaction.html) section.

In addition to the basic attributes that define the character's traits, each character maintains a set of *dynamic attributes* during their interactions with the environment. These dynamic attributes store the character's current state, historical behaviors, and other context-related information.

## Attributes

### Basic Attributes

The *basic attributes* represent the inherent characteristics of a character, providing a multi-dimensional description through a tagged information approach. In this project, the basic attribute template for each character is as follows:
```json
{
    "id": "<int>",
    "gender": "<str>",
    "age": "<int>",
    "marital status": "<str>",
    "country/region": "<str>",
    "education": "<str>",
    "occupation": "<str>",
    "personality": "<str>",
    "interests and hobbies": ["<str>"],
    "consumption tendency": "<str>"
}
```
Each attribute item includes multiple selectable values or value ranges:
- `gender`: Options include male and female.
- `age`: Integer range from 20 to 60.
- `marital status`: Options include single, married, divorced, widowed, separated, in a relationship, and other.
- `country/region`: All countries are supported, represented by country names, following ISO 3166-1 standard.
- `education`: Options include none, primary school, high school, vocational training, bachelor's degree, master's degree, doctorate, and other.
- `occupation`: Options include student, teacher, engineer, doctor, nurse, unemployed, manager, salesperson, service worker, construction worker, self-employed, government employee, and other.
- `personality`: Represented using the MBTI (Myers-Briggs Type Indicator) framework, which classifies individuals into one of 16 personality types (e.g., INTJ, ESFP, INFP).
- `interests and hobbies`: Options include sports, music, reading, traveling, gaming, cooking, technology, art, fitness, gardening, fashion, movies, socializing, other.
- `consumption tendency`: Options include frugal, moderate, impulsive, and other.

Each character's basic attributes follow the rules outlined in this template. In practice, the basic attributes for each character are obtained through uniform sampling within the specified ranges.

### Fixed Attributes

*Fixed attributes* refer to the attributes of a character that need to be instantiated and determined for a specific scenario (i.e., within a single day's simulation). These attributes are set before the simulation begins for the day and remain unchanged throughout the simulation process.
```json
{
    "relationship_id": "<int>",
    "group_id": "<int>",
    "perception_range": "<float>",
    "add_time": "<str>"
}
```
- `relationship_id`: Represents the relationship connection of a character. Characters with the same `relationship_id` are considered to have a connection and are capable of interacting with each other. Can be set to `None`.
- `group_id`: Indicates whether a character belongs to a specific group and which group it belongs to. Characters with the same `group_id` will act together. Can be set to `None`.
- `perception_range`: The perception range of the character, within which the character can sense information about the states present in the environment.
- `add_time`: The time at which the character is added to the environment.

Fixed attributes are determined when a character is initialized and are set through random sampling.


### Dynamic Attributes

*Dynamic attributes* represent the real-time changing information of each character while acting within the scene. They include:
```json
{
    "position": [],
    "current_state": [],
    "executing_action": [],
    "behavioral_trajectory": []
}
```
- `position`: `[x, y, floor]`, representing the current location of the character.
- current_state: `[state_id, "in"]`, `[state_id, "out"]`, or `[None, None]`, representing whether the character is inside a certain state, outside a state, or not in any state at all.  
  The current_state changes as the character executes new actions: "in_state" type actions move the character into a state; "out_state" type actions move the character outside a state; "transition" type actions first remove the character from all states, then place it outside a target state.
- `executing_action`: `[action_args, start_time]` or `None`, representing the action the character is currently performing and the time it started. If no action is being executed, this is `None`.
- `behavioral_trajectory`: `[[current_state, observation, action_args, feedback, [start_t, end_t]], ...]`, representing the historical behavior trajectory of the character. Each entry contains:  
  - `current_state`: The state the character is currently in.  
  - `observation`: Information about the surrounding states. If the character is in a state, this is the information of that state; if not, it includes all states within the character's perception range.  
  - `action_args`: The action the character is performing.  
  - `feedback`: Simulator feedback after executing the action.  
  - `[start_t, end_t]`: The time interval during which the action was executed.

The dynamic attributes of a character are updated whenever the simulator provides new information. These attributes are synchronized periodically as the simulation progresses. Note that, although each point in the character's behavioral trajectory contains only the information mentioned above, the character's decisions are not solely based on its observations and current state. They also rely on the character's **memory**. By drawing from both historical and current information stored in the character's memory, a **context** is constructed, representing the data available to the character that influences its current decision-making.

Before the simulation starts, each character to be added to the environment undergoes instantiation and initialization. The basic attributes of the character are generated through random sampling within specified ranges for each attribute. Fixed attributes are also determined through random sampling (with varying sampling methods). These attributes remain unchanged throughout the simulation for the day. Meanwhile, the dynamic attributes are initialized before the simulation begins and are progressively updated as the simulation unfolds.

## Context

The **Context** represents the environmental and information accessible to a character in its current situation. It serves as the foundation that influences the character’s decision-making.
```json
{
  "memory": [],
  "current state": [],
  "current observation": []
}
```
- `memory`: The simplified behavioral memory of the character.  
- `current_state`: The current state and status of the character.  
- `current_observation`: The current observation of the character (information about surrounding states).

Together, these factors shape the character's next behavior.

For each character, the simulation process follows an iterative loop:
```
add_character(update)
obtain_observation(update) -> generate_context -> get_action -> action_start(update) -> action_complete(update)
obtain_observation(update) -> generate_context -> get_action -> action_start(update) -> action_complete(update)
...
```

## TBD
1. A completely random sampling of character parameters may not align well with real-world scenarios. For instance, a character's gender and age are often correlated with other attributes such as marital status, education level, and interests.
2. The timing of when a character enters the scene needs to be considered. If the entry is determined through random sampling, there could be an issue with the visitation probability at different times of the day. For example, the likelihood of someone visiting a shopping mall is higher in the morning and afternoon, while it's lower just before closing. Uniform sampling fails to account for this variation and could lead to unrealistic scenarios, such as a character entering the mall just one minute before closing.
