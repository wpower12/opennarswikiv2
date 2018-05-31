> # Tour - Plugin System
> A high-level overview of the plugin system.

The major elements of the NARS are implemented in a modularized way. The control, inference engine, and interface components all interact through the plugin system by registering to, responding to, and emitting events. The plugin system allows additional functionality to be quickly added to the system. These plugins can be enabled or disabled at runtime, to allow users to cater the agent to their specific needs. This document will walk through the relevant parts of the code base, illustrating how the plugin system works. 

## Events
The events that the plugin system manages come in many different types. Some correspond to the manipulation of tasks, concepts, and beliefs. While others correspond to the functional workings of the system itself, like a plugin being loaded or a inference cycle ending. These events are more fully described in the Event component page. For the sake of this document, events should be considered the primary 'glue' tying the workings of the NARS components together. 

Events allow disparate parts of the system to communicate with each, and react to each others changes in state. The core of this is the storage.Memory class. This class manages the knowledge base, and contains a reference to an io.events.EventEmitter. 

TODO - link to event component page.
TODO - link to Task, Concepts, Beliefs component pages

## Enabling a Plugin
All plugins must implement the Plugin TODO-ADDLINK interface. This interface provides two methods, one to get the name of the plugin, and one that is called when the plugin is enabled. This method passes in an instance of a Nar agent, which can be used by the plugin to save a reference and interact with the system. 

If a plugin wishes to interact with the Event system, it must also implement the EventObserver interface. This interface provides a single method, event, that allows an observer class to respond to raised events. Plugins may interact with an arbitrary subset of the set of events. The plugins specific definition of the event method allows it to choose what to do in response to specific event types. This is done by reflection on the class variable passed to the event method. 

The Nars instance memory maintains a reference to an EventEmitter. This manages a set of references to EventObservers. A plugin is ‘attached’ to the system by being set on with the EventEmitter.set() method. The EventObserver class is passed in, along with a boolean indicating whether or not to enable to plugin, followed by a list of the event classes that the Observer wishes to ‘react’ to. 

To raise, or emit an event, the plugin class must use its retained reference to the Nars memory object. This provides an emit method that allows the plugin to send events into the system as it sees fit. 

When the appropriate events occur, the Emitter uses its collection of Observers to identify those that would like to react to the event. It then passes the event class onto the EventObserver.event() method, allowing it to react. 

A good example of the process is in the main.Plugins class, which is used to load a ‘standard’ set of plugins for use in a NARS agent. We can see that the init method within the class uses a method on the NARS agent, addPlugin. 

Add plugin does two things. First if the plugin is a type of new operator, it is specifically added to the memory through an addOperator method. Then, the plugin is also added to a list of objects that represent the state of plugins, PluginState.

For an Agent to do something akin to reasoning, it must include a control plugin. As of this documents writing, the codebase mainly uses one; the InternalExperience plugin. This contains a reference to a DerivationContext. Together, these classes form the inference engine that is mentioned in the overview document. 

TODO - Add links for InternalExperience, DerivationContext, Overview Document

## Plugin Guidelines
When adding a plugin to the system, it should be done under a set of common assumptions. For a plugin to be considered valid, it must have the following properties.

* They are not allowed to violate NAL, if plugins generate NAL statements by other NAL statements, the plugin has to apply valid NAL rules and manage the stamp correctly. For example when a planner-plugin helps the general control mechanism to generate a plan to fulfill a desired goal, the resulting plan will have the same truth-value as the general control mechanism would have created with temporal induction and the temporal chaining rule.
* If they need the system to actively call an operator in order to use the plugin (most plugins are that way), they need to provide the system with the needed usage desire evidence when loaded. The system won't use what it doesn't know that it exists. A example for a possible wiki-ask-plugin:

```
<(&/,(^wonder,$sth),(^pluginAskWiki,$sth)) =/> <(*,SELF,$sth) --> knows>>.
(&&,<#sth --> knowledge>,<(*,SELF,#sth) --> knows>)! 
```

* They shouldn't violate AIKR (Assumption of Insufficient Knowledge and Resources). For some application-specific plugins it is okay to violate it though if the entire system should really just operate in this domain.


## Example - InternalExperience Plugin

The inference cycle can be modified by plugins. The previously mentioned InternalExperience is one such plugin. It implements both the Plugin and EventObserver interfaces. 

In the definition of the setEnabled method (implemented to satisfy the Plugin interface), we see that the plugin wants to observe Events.ConceptDirectProcessedTask events. This is done by calling the memory.event.set() method. In doing so, the class passes the set method a reference to itself (since it extends the EventObserver class), the value of the enabled parameter, and the class reference for the event type.

The plugin also defines the event method (to satisfy the EventObserver interface). This method checks the class type of the provided event, and responds if it matches the ConceptDirectProcessedTask type. When this type of event is found, the method passes it on to a method that interacts with actual reasoning systems. The specific behaviour is out of scope for this setting, but more information on the inference cycle can be found here.

TODO - Link to a page on the inference cycle - component page?


## Making your own Plugin

TODO - Checklist style guide for making a plugin - what to implement/extend. how to reflect on the class types.

## Other Plugins

TODO - Link to code/deeper descriptions for these.

### Internal Experience

Allow the system to reason about internal processes, when removed NAL9 will not be used by the system if not especially trained. NAL9 is very important for the development of consciousness. (is activated by default)

***

### Temporal Particle Planner

This plugin is a planner plugin which works by collecting temporal implication statements (=/> etc.), trying to chain them together to find a way with high expectation from currently true statements (they have low cost), to the desired goal by translating the needed intermediate steps into actions which make this intermediate steps true. This happens by sending particles which are guided by priority through this graph. Similar like in an ant colony, an executed path will then be rewarded by the standard control mechanism through temporal induction and revision. When NARS forgets a concept with (=/>) absolutely, it will also be removed in the planner, this way, and due to the fact that the amount of particles and their energy sent in each step is limited, AIKR is preserved. The resulting plan gets executed like the timing suggests, can be updated by newer plans (if the steps have higher priority), and always be interrupted by the general control mechanism. This plugin can help NARS a lot in decision making domains where a lot of planning is needed because it concentrates the spent time on planning. But due to this the time for other mental processes gets reduced, so use this plugin cautiously, since planning can also be done by the standard inference process.

***

### Global Anticipation

This plugin is for accelerating the updating of temporal hypotheses by the strategy, to let all hypotheses over a certain threshold of priority directly watch the input events. When the preconditions are matched, and the predicted event happens, it gets strenghtened (temporal induction), if not, it gets weakened. (When loaded it overwrites the default strategy, which works by generating a negative event when a disappointment of a prediction happens)

***

### Misc Plugins

- Mental Operators - Because especially NAL9 operators may come with parameters, we decided to let operators also inherit from plugin. That way mental operators can be eliminated, added and configured at runtime.

- Runtime Settings - Some runtime parameters are not important enough to be part of the main window, but may be interesting to change sometimes, so we decided to add a little plugin which is just for changing runtime parameters which don't fit into the main window.

***