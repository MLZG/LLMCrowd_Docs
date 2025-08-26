# Environment

## Definition

The *environment* is defined as the 3D layout where crowd simulation takes place, supporting sustainable navigation and interactive behaviors of individuals within it.

In this system, the environment is predefined as a 3-floor shopping mall. It is composed of structured 3D models and includes various types of shops, cafes, and associated details like items for sale, as well as facilities like escalators, elevators, doors, gates, etc. The environment supports multiple interactions between characters and objects, and contains navigable ground structures. This environment is built using Unreal Engine, and a portion of the content is shown below:
<p align = "center">
<img src="../../_static/mall/mall_overview.jpg" width=48% style="margin-right: 5px; margin-left: 5px;">
<img src="../../_static/mall/mall_internal1.jpg" width=48% style="margin-right: 5px; margin-left: 5px;">
</p>
<p align = "center">
<img src="../../_static/mall/stores/mall_phonex.jpg" width=48% style="margin-right: 5px; margin-left: 5px;">
<img src="../../_static/mall/stores/mall_fashion.jpg" width=48% style="margin-right: 5px; margin-left: 5px;">
</p>

This environment originates from [Fab](https://www.fab.com/listings/b1628005-1f64-4833-a076-475ae954daec).

## Structure 

### Macro

From a macro perspective, the shopping mall consists of three floors, with access between floors via escalators, elevators, or stairs, and includes a total of 40 open stores.

All the stores, areas, and facilities within the environment are extracted, with each defined as an *entity*. The entities have the following categories:

`[store, restaurant, area, toilet, lift, escalator, floor, unavailable area, useless]`.

Each entity is an instance of one of the aforementioned categories. For each entity, the following annotations are provided:  
- `id`: Unique identifier.
- `name`: The actual name of the entity within the shopping mall.
- `category`: The category label.
- `description`: A textual description of the entity.
- `boundary`: The coordinates that define the entity's boundary.
- `doors`: Coordinates for entry or exit of the entity (if applicable).

From these annotations, the top-view semantic map of the environment is obtained:  
<p align = "center">
<img src="../../_static/mall_map/topview_floor1.png" width=32% style="margin-right: 0px; margin-left: 0px;">
<img src="../../_static/mall_map/topview_floor2.png" width=32% style="margin-right: 0px; margin-left: 0px;">
<img src="../../_static/mall_map/topview_floor3.png" width=32% style="margin-right: 0px; margin-left: 0px;">
</p>

Among all the entities, those that can be accessed by characters on a semantic level are filtered out. These entities are assigned additional attributes and defined as *states*. All states and their transfer relationships form a structured representation of the environment. For a detailed definition and structure of *state*, please refer to [state](https://llmcrowd.readthedocs.io/en/latest/developing/method_components/state.html).

### Micro

From a micro perspective, internal modeling of each entity is included:
<p align = "center">    
<img src="../../_static/mall/stores/mall_pizzeria.jpg" width=32% style="margin-right: 3px; margin-left: 3px;">
<img src="../../_static/mall/stores/mall_phonex.jpg" width=32% style="margin-right: 3px; margin-left: 3px;">
<img src="../../_static/mall/stores/mall_oldtown.jpg" width=32% style="margin-right: 3px; margin-left: 3px;">
</p>
<p align = "center">    
<img src="../../_static/mall/stores/mall_elec1.png" width=57% style="margin-right: 3px; margin-left: 3px;">
<img src="../../_static/mall/stores/mall_fashion.jpg" width=39% style="margin-right: 3px; margin-left: 3px;">
</p>

Characters in the environment can navigate around (via a navigation mesh), perform specific actions, and interact with objects and facilities, such as looking into a mirror, sitting on a sofa to rest, or using an elevator. These functionalities are specifically defined and implemented in the [Simulator](https://llmcrowd.readthedocs.io/en/latest/developing/simulator/index.html) part.
