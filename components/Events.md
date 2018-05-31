# Component - Events

The OpenNARS implementation is modularized, with plugins communicating via an event system. The following is a brief description of the events that can be subsribed to in the OpenNARS framework. The code defining the event types can be found org.opennars.io.events TODO-Here-link.

## Cycle Events

* CyclesStart    - fired at the beginning of each Nar multi-cycle execution
* CyclesEnd      - fired at the end of each Nar multi-cycle execution 
* CycleStart     - fired at the beginning of each memory cycle 
* CycleEnd       - fired at the end of each memory cycle 
* WorkCycleStart - fired at the beginning of each individual Memory work cycle 
* WorkCycleEnd   - fired at the end of each Memory individual cycle 
* ResetStart     - called before memory.reset() proceeds 
* ResetEnd       - called after memory.reset() proceeds 

## Concept Events

## Executive and Planning Events

