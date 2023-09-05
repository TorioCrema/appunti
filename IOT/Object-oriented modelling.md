# Top-down approach

Starting from the *domain* and its modelling using high-level paradigms, such as OO.

Programming paradigms such as OO are first of all **modelling** paradigms:
- they specify how we con cenpetualise, represent, model a system,
- how we can analyse a problem and define the corresponding solution in some application domain

## Model

The model is the representation of relevant aspects of a system, abstracting from those that are considered not relevant.
The relevant aspects concern the structure, behaviour and interaction of the system.
Strong relationships between modelling and programming.
Modelling allows to analyse the problem and define solutions in a more abstract way than implementation technologies and languages.
Advantages:
- better understanding of system working and management of related complexities
- reusability, potability
- extensibility
- allows to have more rigorous approaches to verify the correctness of the system

### Foundamental dimensions

- Structure: what parts compose a system
- Behaviour: the computational behaviour of every part
- Interaction: how parts interact

### Paradigms

Modelling paradigms are a set of concepts and principles defining models, often related to programming.
Modelling languages provide a rigorous non abiguous way to represent models, they are ofter related to specific modelling paradigms.

### Object Oriented Paradigm

Main paradigm in modelling and programming software, it's effective level of abstraction allow to capture and represent essetial aspects of the problem and its domain.
Fostering important properties at the design and programming level:
- modularity
- encapsulation
- reuse and extensibility

Among important aspects not directly captured by the paradigm are concurrency, asynchronous interactions and distribution.

### Modelling embedded software based on micro-controllers

Two main macro parts:
- controller:
	- encapsulating the control/application logic concerning the task(s) to do
	- to this purpose, it accesses/uses/observes passive resourses and devices
	- active component
- controlled elements
	- modelling the resources and devices managed/used by the controller to do its job
	- encapsulates functionalities useful for the controller
	- passive component

#### Features

- Controller:
	- Integrating pro-active and re-active behaviour
	- based on a control loop architecture (super-loop, event-loop)
- Controlled elements:
	- well-defined interface
	- observable state and events
	- reusability, controllability
	- OO model

### Control loop and finite state machines

Execution model based on a control loop (or super loop):
- at each cycle the controller reads the input state and chooses the proper action to perform
	- the choice of the action depends on the current **state**
	- the action can update the current state
- There's a need for proper formalisms to describe state-based behaviours: **Finite State Machines (FSM)** synchronous or asynchronous.

#### Efficiency and reactivity

- Efficiency: the loop is always cycling even if the controller is conceptually waiting for an input
- Reactivity:
	- depends on how fast a cycle is completed
	- if during a single cycle sensors change states multiple times, these changes are not detected

### The controller as an active entity

About the conceptual nature of the controller:
Objects in OO modelling are typically passive entities and they don't encapsulate a control flow, while the controller is an active entity, with its own logical controw flow; at the design level it is meant to encapsulate the control strategy of the system.

#### Agents

Agents are first-class active entities equipped with an autonomous logical control flow, designed to achieve or perform some task(s) that call for being reactive to data and events perceived from the environment, and that do actions to have an effect on the environment through some actuators.
- reactive + pro-active behaviour
- encapsulating a control flow
- task-oriented design

### Task-based decomposition and execution

Complex jobs can be decomposed into tasks, that are executed by the control loop:
- the agent carries on one or multiple tasks concurrently
- the behaviour of each task can be described by a state machine

#### Multi-agent systems

Complex embedded systems can be distributed and actually include a system of interacting/cooperating indipendent sub-systems.
In order to model these systems we can consider multi-agent systems, as enseble of agents that interact and cooperate.