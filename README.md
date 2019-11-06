Lehn's Architectural Description Language
=========================================

This is my favorite architectural description language.
It allows to model (de-)composition structures of system architectures.


Basic concepts
--------------

The basic concepts are
 * **Component**: The building blocks of a system, comparable to classes in an object-oriented programming language.
 * **Object**: A part of a system.
 * **Message**: Information send from one object to another.
 * **Port**: The end point of communication links between objects.
 * **Communication Link**: A connection link between two object ports to exchange messages.

Each entitiy mentioned above (except communication links) has an identifier as name.
An object can send a message over a port to another object,
that is connected to this port.
Vice versa, objects receive messages from other object through ports.
For each port it can be specified,
which input and output messages can be sent or recieved over that port.
(Interface are currently not part of this architectural language but it may be extended.)

The statements

    message b, c, d
    port a in b out c, d

specifies `a` port which receives messages `b` and sends out message `c` and `d`.

With the following statements a component `e` with this port is specified:

    component e {
        port a in b out c,d
    }

There is exably on port `a`.

The following example shows the definitions of some components.

    message transportOrder
    message transportStatus

    component transportVehicle {
        port control
    }

    component controlUnit {
        port vehicle*
        port backend
    }

    component transportSystem {
        port backend in transportOrder out transportStatus    
        part transportVehicle vehicle*
        part controlUnit

        backend -- controlUnit.backend
        vehicle*.control -- controlUnit.vehicle*
    }

The `transportSystem` has a port `backend` and two parts: `vehicle` and `controlUnit`.
`vehicle` is an object of type `transportVehicle`.
`controlUnit` is an object of type `controlUnit`.
It is named implicitly with the name of the component,
because no name is specified for that part.
Communication links are establishe between:
 * the `backend` port of the component and the `controlUnit`
 * the `garage` port of `transportVehicle` and the `vehicle` port of the `controlUnit`

Ports and parts are instanciated.
The _multiplicity_ modifiers `*`, `+`, `?` specify how often the entitiy is instantiated.
If there is no modifier the entity is instanciated exactly once.
`?` means that the entity is optional.
That means, it is instanced zero or one time.
Components marked with `+` and `*` are instantiated many times,
with `+` at least once.
`[<n>]` expresses that the entitiy is instantiated exactly <n> times.


Functions
---------

Components realize functions.
The functions use input messages and deliver output messages.
They can be specified by the functions of the underlying components in a functional design.


Failure
-------

Failures are situation in which a component is not able to fulfil its functions as specified.
Analysing the causes and effects of failures will lead to an [FMEA][2].


Statemachines
-------------

Statemachines are the fundamental concepts to specify real time system.
A very good example how to do this is the [Real-Time Object-Oriented Modelling][1] (ROOM) by Bran Selic et. al.


Views
-----

Views are diagrams that show some aspects of the architecture graphically.
With views you can explain a system in architectural documents.
Views can be generated on base of the model information.
The most promising approach is to generate PlantUML or DOT files from the model files.


No-no: Code Generation
---------------------

I will never ever generate code from architectural models.
But may be the opposite is sensible: Generate `ladl` models out of code.


[1]: https://www.amazon.com/Real-Time-Object-Oriented-Modeling-Bran-Selic/dp/0471599174
[2]: https://en.wikipedia.org/wiki/Failure_mode_and_effects_analysis