Lehn's Architectural Description Language
=========================================

This is my favorite architectural description language.
It allows to model (de-)composition structures of system architectures.


Basic concepts
--------------

The basic concepts are
 * **Component**: The building blocks of a system, comparable to classes in an object-oriented programming language.
 * **Object**: The parts of a system.
 * **Message**: Information send from one object to another.
 * **Port**: The end point of communication links between objects.
 * **Communication Link**: A connection between object to exchange messages.

Each entitiy mentioned above (except communication links) has an identifier as name.
An object can send a message over a port to another object,
that is connected to this port.
Vice versa, objects receive messages from other object through port.
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
`vehicle` in an object of type `transportVehicle`.
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
That means it is instanced zero or one time.
`+` and `*` means that the component is instantiated many times.
With `+` at least once.
`[<n>]` expresses that the entitiy is instantiated exactly <n> times.


Functions
---------

Components realize Functions.
The functions use input messages and deliver output messages.
They can be specified by the functions of the underlying components in a function design.


Failure
-------

Failures are situation in which a component is not able to fulfil its functions as specified.
Analysing the causes and effects of failures will lead to an FMEA.


Statemachines
-------------

Statemachines are the fundamental concepts to specify real time system.
A very good example how to do that is the Real-Time Object-Oriented Modelling language (ROOM) by Brian Selic.


Views
-----

Views are diagrams that show some aspects of the system graphically.
With views you can explain a system in architectural document.
Views can be generated on base of the model information.
The most promising approach is to generate PlantUML oder DOT files out of the models.


NoNo: Code Generation
---------------------

I will never ever generate code out of the architectural model.
But may be the opposite is possible: Generate `ladl` models out of code.
