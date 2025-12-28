# Scenario

## Definition

A *scenario* is defined as a full-day simulation scene on a specific date. It encompasses both the status evolution of all states and the entire set of actions performed by all characters within that day. In this project, a total of 365 scenarios are provided, representing each day of the year (2026). Each scenario has its own attributes and components, which are initialized with distinct configurations to reflect variations across different days.

## Scenario Attributes

Each *scenario* is associated with a set of fixed attributes:

```json
{
    "date": "[year, month, day]",
    "weekday_num": "<int>",
    "weekday_name": "<str>",
    "season": "<str>",
    "holiday": "<str>",
    "weather": "<str>",
    "temperature": "<int>",
    "customer_total_num": "<int>"
}
```
- `date`: The specific day of the scenario, represented as `[year, month, day]`.
- `weekday_num`: The numeric representation of the weekday (e.g., Monday = 0, Sunday = 6).
- `weekday_name`: The name of the weekday (e.g., "Monday").
- `season`: The season corresponding to the given date.
- `holiday`: A string indicating whether the date is a holiday or special occasion.
- `weather`: Weather conditions on that day (sampled randomly).
- `temperature`: The temperature of the day (sampled randomly).
- `customer_total_num`: The total number of customers expected on that day (sampled randomly).

Attributes such as `weekday`, `season`, and `holiday` can be derived from the `date`. Meanwhile, `weather`, `temperature`, and `customer_total_num` are determined through random sampling.

In addition to fixed attributes, each scenario maintains certain dynamic attributes during the simulation process, including:
- `current_customer_num`: The number of customers currently present in the scene.
- `current_time`: The current simulation time within the day.

## Scenario Components

A scenario incorporates nearly all the predefined components, including:

- **State Component**  
  Responsible for maintaining the overall states structure and its associated attributes. It updates the set of states according to the progression of the scenario simulation.

- **Character Component**  
  Responsible for maintaining all characters within the scene and their related attributes. It updates the internal status of all characters as the scenario simulation evolves.

- **Action Component**  
  Responsible for maintaining the pool of all available actions in the scene, as well as the methods for action instantiation.

The State Component can be regarded as the environment structure of the scenario, while the Character Component (a list of characters) can be regarded as all pedestrians in the scenario and their behavioral records. The Action Component serves as a static action library. Together, these components—combined with the attributes of the scenario—form a complete and functional scenario.

## Scenario Initialization and Updates

When a scenario is created, it undergoes an initialization process. This step initializes both its own attributes and all of its associated components. After initialization, as the simulation progresses and the pipeline advances, the scenario performs periodic maintenance and updates. The fixed attributes of the scenario remain unchanged throughout the simulation, while the dynamic attributes and components are updated according to feedback from the simulator.

## Issues

1. **Initialization of Customer Numbers**  
   The number of customers in a scenario should not be sampled purely from a uniform range, as it is highly dependent on the scenario attributes. For example, on a holiday, the customer volume is likely to be higher; On a rainy day, the customer volume may be lower. If we rely solely on random sampling, these correlations cannot be captured. Therefore, whether the attribute `customer_total_num` should be included in random sampling requires further consideration.

2. **Daily Parameters Sampling**  
   Certain daily parameters, such as *weather* and *temperature*, also cannot be realistically sampled in a fully random manner. These attributes have strong temporal correlations with the date. Weather and temperature do not typically fluctuate drastically in a short span (e.g., a sudden shift from very cold today, to very hot tomorrow, and then back to very cold the day after). Similarly, weather patterns rarely alternate too erratically (e.g., sunny → rainy → sunny → rainy over consecutive days). Fully random sampling would create unrealistic daily fluctuations, leading to scenarios that lack credibility.


