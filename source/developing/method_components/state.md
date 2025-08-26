# State

## Definition
A **state** is defined as an entity within the environment that a character can access semantically. It includes various stores, shops, public areas, and more. Each state inherits the basic attributes of an entity, such as id, name, category, description, and location, and also maintains additional attributes to enrich its information and represent its internal status.

All states are mutually accessible, meaning any two states can transition between each other. If states are treated as
accessible nodes within the environment, a directed complete graph formed by all states can be seen as a structured
representation of the environment.

## Scope
The category of **state** is limited to [store, restaurant, area, toilet]. Entities such as [lift, escalator, floor, unavailable area, useless] are excluded from being considered as states. See [Environment](https://llmcrowd.readthedocs.io/en/latest/developing/method_components/environment.html) for more details of the entities.


## Attributes
Each state maintains several attributes, which are divided into three categories: **basic attributes**, **daytime attributes**, and **real-time attributes**. The basic attributes remain constant, the daytime attributes are fixed when the state is initialized to a specific date, and the real-time attributes change dynamically as the simulation progresses.

### Basic Attributes
Each state has the following basic attributes:
```json
{
    "state id": "<str>",
    "state name": "<str>",
    "state category": "<str>",
    "state description": "<str>",
    "supported actions": ["<str>"],
    "boundary": ["lists of float"],
    "main_doors": ["lists of float"],
    "sub_doors": ["lists of float"],
    "other_doors": ["lists of float"],
    "state location": ["<float>", "<float>", "<str>"]
}
```
The basic attributes of a state inherit from the corresponding entity's attributes. The state location is preprocessed from the boundary and is represented as [x, y, floor]. The x and y coordinates are determined using Vladimir Agafonkin's polylabel algorithm to compute the centroid of the state's polygon. Additionally, each state has a predefined set of supported actions, representing the actions that characters can perform within that state.

### Daytime Attributes
Each state contains some **daytime attributes** that remain constant throughout a given day, once the date is determined. These attributes are fixed for the day:
```json
{
    "max_capacity": "<int>",
    "opening_hours": "<str>",
    "event": "<str>"
}
```
- Maximum capacity: The maximum number of people the state can accommodate.
- Opening hours: The opening and closing times for that day.
- Event (if applicable): Whether any special events are taking place that day and details of those events.

### Real-time Attributes
**Real-time attributes** represent the dynamic properties of a state that change throughout the day as the simulation progresses.
```json
{
    "current_human_num": "<int>",
    "current_queue_count": "<int>",
    "current_open_status": "<bool>",
    "current_event": "<str>"
}
```
- Current human num: The number of people currently in the state.
- Current queue count: The number of people currently waiting in line.
- Current open status: Whether the state is open or closed.
- Current event: Whether there are any ongoing special events and their contents.

In summary, the **basic attributes** of the entire state collection remain constant. **Daytime attributes** are set based on the date and remain unchanged throughout the simulation of that day. **Real-time attributes** are dynamically updated based on the current simulation state of the shopping mall. 

Note that, during the simulation, the LLM can retrieve all basic and daytime attributes of each state in advance. However, the real-time attributes, which change dynamically as the simulation progresses, can only be accessed through the character's observations. These real-time attributes are incorporated into the character's observations and serve as part of the local context, helping to guide decision-making.
