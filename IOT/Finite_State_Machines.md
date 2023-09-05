# Models for Embedded systems

Embedded systems models include both discrete and continuous components.
Continuous components evolve smoothy, while discrete components evolve abruptly, with ~anatomic changes.
The physical dynamics of the system can often be modeled with ordinary differential or integral equations.
Discrete components featuring discrete dynamics can be effectivly modelled by state machines.

## Example

Consider a system that counts the number of cars that enter and leave a parking garage in oder to keep track of how many cars are in the garage at any time.
It can be modelled by three main interacting subsystems:
- ArrivalDetector: produces an event when a car arrives
- DepartureDetector: produces an event when a car leaves
- Counter: keeps a running count, starting from an initial value $i$. Each time the count changes, it produces an output event that updates a display.

In this example each entry or departure is modelled as a discrete event.

### Counter

The *Counter* subsystem reacts to the sequence of input events and produces an output event:
- the input is represented by a pair of discrete signals (up/down) that in some specific moments may have an event.
- the output is a discrete signal that, when the input is present, has a certain value represented by a natural number, while in other moments is absent.

When an event is present at the up/down port, *Counter* increments/decrements the count and produces its value in output, in all other cases the output is absent.

### Input and outupt signal model

The $u$ and $d$ signals (up and down) that represent the occurence of events can be modelled as these functions:
$$u:\mathbb{R}\rightarrow\{\text{absent, present}\}$$
At each time $t$, the signal $u(t)$ is evaluated either as "absent" (no events at that moment), or "present", meaning that there is an event at that moment; these signals are called "pure" because they don't carry any information content but define presence or absence.
The output signal $c$ can be represented by a function:
$$c:\mathbb{R}\rightarrow\{\text{absent}\}\cup\mathbb{N}$$
This is not a pure signal: it carries an information content on top of being present or absent.

#### Reactions

The dynamics of a discrete system like in this case can be described as a sequence of steps called **reactions**, being each reaction instantaneous (with a duration equal to zero).
The reactions of a discrete system are triggered by the environment where the system works:
- when they are triggered by input events they are called event-triggered

The execution of a reaction leads to a **valuation** of the inputs and outputs, that is:
- a variable is associated to each input signal and the valuation of the input consists in assigning the value of the input signal at that time to the variable
- the same applies for output signals: for each output signal we use a variable that is assigned to the value of the output function in that moment
- the values can include also "absent", meaning no signals.

### The notion of state

The state of a system is its condition at a particular point in time, and in general, the state affects how the system reacts to inputs.
Formally, we define the state to be an encoding of everything about the past that has an effect on the system's reaction to current or future inputs: the state is a summary of the past.
The Counter subsystem in the example, $\text{state}(t)$ - i.e. the state at time $t$ - can be represented by an integer in the range between $0$ adn $M$, where $M$ is the max number of parking spaces.
Given the set $\text{States}=\{0,1,\dots,M\}$, then the state can be modelled as the function:
$$\text{state}:\mathbb{R}\rightarrow\text{States}$$

## Finite State machines

A **state machine** in general is a model of a system with discrete dynamics that at each reaction maps valuations of the inputs to valuations of the outputs, where the map may depend on its current state.
A **finite-state machine** is a state machine where the set States of possible states is finite.
If the number of states is reasonably small, the FSM can be conveniently drawn using state diagrams.

### Transitions

**Transitions** between states govern the discrete dynamics of the state machine and the mapping of input valuations to output valuations.
- a transition is represented as a curved arrow, going from one state to another
- a transition may also start and end at the same state, in this case, the transition is called a self transition.

Transitions are characterised by a **guard** and an **action**:
- the guard determines whether the transition may be taken on a reaction, it is represented by a predicate (a boolean-valued expression) that evaluates to true when the transition should be taken, when a guard evaluates to true we say that the transition is enabled
- the action specifies what outputs are produced on each reaction, it is an assignment of values (or absence thereof) to the output ports, any output port not mentioned in a transition that is taken is absent

### Asychronous and synchronous FSM

A FSM does not specify when valuation shoud be carried on to trigger the reaction.
There are two possibilities:
- Asynchronous FSM, also called Event-triggered FSM, in this case, the valuation occurs anytime there is an input event, the environment where the system works establishes when the reactions are evaluated and triggered
- Synchronous FSM, valuations occur periodically, at a regular interval of time called period, the period determines the working frequency/rate of the machine.

### Input/Output modelling

When applying FSM to embedded systems, the input/output are then mapped variables that are used to define guards and actions:
- input variables are modified by the external environment where the machine operates
- output variables are modified by the machine by means of actions, to control the external environment

Besides I/O, variables can be used to flexibly define the global state of the machine.
Guards in transitions are defined using these variables (both input and state variables)

### Implementing A FSM

General schema to implement a FSM as a program controlling the embedded system behaviour.
Full process from design to code:
- first the FSM is defined, representing a model of the behaviour (design state)
- then it is converted/implemented into a program to be run on microcontrollers

In model-driven engineering, this conversion is automated by means of proper tools.

```c
/* global vars used by the machine */
int a0, b0, …
/* variable keeping track of the state */
enum States {…} state;
/* procedure implementing the step of the state machine */
void step(){
	switch(state) {
		case … 
		case … 
	} 
}

loop(){
	step();
}
```

In a correct FSM model and implementation:
- the computations related to actions should always terminate, there should be no infinite loop, in theory they should be instantaneous

The valuation of a condition should not change the state of variables.

### Time-triggered FSMs

The behaviour of an embedded system is typically **time-oriented**, time is often part of the specification in guards and actions (e.g. as deadlines), as well as to define regular/periodic behaviours.
Synchronous FSM have been introduced for this purpose, to provide an effective way to handle time and its passing, to ease the specification of time-oriented behaviours.
- in this machine, the step of the machine is executed periodically, considering some specified **period**.
- it is like that FSM has an internal *clock* and transitions occur only at the clock tick.

Programmable timers are typically used to implement time-triggered FSM, programmed to generate an interrupt at the desired frequency (with a specified period), the interrupt then should lead to execute a step of the machine.

The general stategy is to consider the greatest common divisor (GCD) as period of the machine.

#### Intup Sampling

**Sampling** is a term used to refer to the periodic reading of sensors, at some frequency (i.e. with some period):
- sampling rate = frequency of the periodic reading, sampling rate = $\frac{1}{\text{period}}$
- measured in Hertz (Hz)

**Choosing the period is a critical choice in the design of a synchronous FSM:**
- the period should be small enough not to loose events
- the period should be big enough as to avoid overrun exceptions

The greater the frequency (i.e. the smaller the period) the higher the reactivity, on the other hand the bigger the use of the microcontroller and the power consumption.
Then as a general stategy, the period should be the greatest value of that range of values that guarantees not to loose events and so the correct functioning of the system.

The **minimum event separation time** (MEST) is defined as the smallest interval of time that can occur between two input events, given the environment in which our system is operating.
**Theorem**: in a sync FSM, by choosing a period lees than MEST, the we have the guarantee that all events will be detected, i.e. no events will be lost.

#### Latancies

Some input events may be required to generate some output events or actions, within a certain amout of time.
The interval between the occurrence of the input event and the corresponding event and corresponding generation of the output event is called **latency**.
A typical objective in embedded systems is to **minimise latencies**, there is a relationship between the period and latencies, the smaller the period the smaller will be the latency.

#### Input Conditioning

Sensors in general can be affected by physical or HW imperfections, so that we need to apply some **input conditioning** in order to avoid errors in sampling the input signals.
A common example is the **bouncing** of tactile buttons. (A single pressure of the button can lead to multiple input signal switching between LOW and HIGH before settling on HIGH)