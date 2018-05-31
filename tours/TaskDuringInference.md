> # Following a Task Through Inference
> Understanding the processing of tasks within the inference engine.

This document will walk through the code that implements the inference engine of the NARS theory in OpenNARS. The inference engine is one of the three core components of an agent, along with Memory and Control. Its job is to take in statements and beliefs, and process them in combination with the current memory, in order to produce new, derived tasks and beliefs. This process is guided by the logical rules provided to the system. TODO-DISCUSSTHERULESSYSTEM

## Overview

In the OpenNARS implementation, the core inference control logic is held by the Memory class. The logic is split between three external classes:

* GeneralInferenceControl
* TemporalInferenceControl
* DerivationContext

Some inference engine logic also exists within the Memory class. Specifically the localInference method. Its workings will be touched on later, but the end result is the execution of code within the TemporalInferenceControl class. 

The first two classes, the inference control classes, are what the Memory class uses to interact with the inference engine. The memory class does this in its cycle TODO-ADDLINK method. A call to this method represents one working cycle of inference within the system. During this call, three things occur related to the inference engine. 

* New Tasks are processed. 
* Novel Tasks are processed.
* A concept is selected for general inference.

The first two methods eventually call on the localInference method. We will first focus on the behaviour of the third method, the GeneralInferenceControl.selectConceptForInference() method. 

When a NARS agent has a new task, and it is added to the system, whether through a user input, through the reading of a file, or due to the result of an inference cycle, eventually the task will make its way to one of Control methods. The following is a description of the selectConceptforInference method, and its role as one of these Control methods. 

## General Inference Control 

This method selects a concept from the provided memory, creates a DerivationContext for the concept and associated memory, and then "fires" the DerivationContext off. Firing a concept invokes a chain of three methods, which culminates in a call to the RulesTable.reason() method. TODO-ADDLINK This method is where the workings of the system finally meet the rules and logic of the NARS theory. Before we describe this, here is the chain of method calls leading up to the reason method. Again, this logic can be found in the GeneralInferenceControl class. TODO-ADDLINK.

* selectConceptForInference(Memory,NarParameters) - Selects concept. Builds Derivation Context. Then calls fireConcept.
* fireConcept(DerivationContext, numTaskLinks) - Iterate over n many Task Links. For each, check the linked tasks budget. If high enough, "fire" the TaskLink.
* fireTaskLink(DerivationContext, termLinks) - Iterate over n many TermLinks related to the task. For each, update the passed DerivationContext and then pass them onto the fireTermLink method. 
* fireTermLink() - Updates the passed in DerivationContext by setting the current concept to the TermLink, and then the DerivationContext and the current concept are finally passed onto the reason method. 

Each of these steps is enclosed by calls to the emit method in the systems memory, sending events to the system indicating the stages of the inference process.

## RulesTable

The RulesTable class contains much of the logic for the execution of reasoning within the system. Its method reason() is its main interface to the rest of the control system. Much of the logic in reason() is related to preparing parameters for a call to the applyRuleTable() method TODO-ADDLINK. 

TODO - List and explain the parameters.
* tLink
* bLink
* nal
* task
* taskSentence
* taskTerm
* beliefTerm
* belief

The applyRuleTable method "dispatches", or routes, to different rule handlers based on two values; the type of the TaskLink, and then on the type of BeliefLink. When the correct handler is found, it is passed the required parameters and the flow continues. The RulesTable class contains most of the required rules; including but not limited to the syllogistic rules, structural inference rules, and conditional inferenec rules. 

## Example - syllogisms

The early levels of the NAL theory consider the basic syllogistic inferences. That is; inheritance, similarity, implicaiton, and equivalence. The actual execution of these inferences occurs in the syllogism() method. It uses the types of the provided task and belief terms to dispatch to methods for the correct type of relation.  TODO-ADDLINK


