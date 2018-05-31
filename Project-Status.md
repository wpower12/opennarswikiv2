> ### Project Status  
> The current status and the plan for the near future of the project.
***

### Current Situation

The most recent releases (after version 1.6.0) have implemented the whole Non-Axiomatic Logic (NAL 1-9), as described in [Non-Axiomatic Logic: A Model of Intelligent Reasoning](http://www.worldscientific.com/worldscibooks/10.1142/8665).

Another major release is OpenNARS 1.5.7, a stable implementation of NAL 1-6, with a simpler user interface.

***
### Future Tasks

In the near future, there are the following tasks to be accomplished, probably in parallel.

####Conceptual design

Though the major design decisions have been implemented, there are still remaining conceptual issues. 

For the logic part, the remaining issues are listed in [OpenNars One Dot Six](https://github.com/opennars/opennars/wiki/OpenNARS-One-Dot-Six); for the control part, they are in [Inference Control](https://github.com/opennars/opennars/wiki/Inference-Control).

The conceptual issues usually cannot be resolved as software development, but should be addressed in ways that are consistent with the overall design of NARS.

####Testing and debugging

In the near future, the focus of testing will still be on the logic part, that is on the expressive power of Narsese and the inferential power of NAL. The hypothesis to be tested is that the language and the logic are complete with respect to the objective of A(G)I.

There are several types of testing cases:

* [Single-step cases](https://github.com/opennars/opennars/wiki/Single-Step-Testing-Cases) show the premise-conclusion relationship of each inference rule. The objective is to systematically document the inference steps that can be carried out in the system, so that all future problems, no matter how complicated, can be analyzed as a composition of these steps. These cases are also needed for debugging after each major change in the design. In these cases, the control part is completely omitted. Now the cases for NAL 1-6 are stable, but NAL 7-9 cases are not.

* [Multi-step cases](https://github.com/opennars/opennars/wiki/MultiStep-Examples) show typical functions of the system that takes several steps. Here the focus is still in the logic part, though the control part is involved, too. The main purpose of these cases will be for demonstration and publication, so the selected cases should be natural and representative, while remain simple. Current considerations include: (1) [Procedural Learning](https://github.com/opennars/opennars/wiki/Procedural-Learning), including classical conditioning and the learning of loops and recursions; (2) Everyday examples, as those discussed in [Surfaces and Essences: Analogy as the Fuel and Fire of Thinking](http://www.amazon.com/Surfaces-Essences-Analogy-Fuel-Thinking/dp/0465018475) and some other literatures; (3) CogSci examples, as those discussed in [Intelligent Reasoning on Natural Language Data: a Non-Axiomatic Reasoning System Approach](http://www.cis.temple.edu/~pwang/9991-PJ/Reports/OzkanKilicThesis.pdf).

* [Application programs](https://github.com/opennars/opennars/wiki/Application-Programs) and [Testing reports](http://www.cis.temple.edu/~pwang/demos.html). Here the objective is to test the overall capability and performance of the system in a specific domain. To carry out such a test, it is important to remember that it cannot be done by simply dump a large number of tasks into the system then let it run. It is more efficient to do it in a step-by-step manner, and to follow the order of (1) Narsese (i.e., make sure all the problems and solutions can be expressed), (2) NAL (i.e., make sure all the desired steps are supported by existing rules), and (3) NARS (i.e., make sure the complete process can be carried out when given proper experience).

####Software development

Beside debugging and improving the design according to software engineering considerations, major development tasks include: 
* Database connection, as explained in [Data And Knowledge Bases](https://github.com/opennars/opennars/wiki/Data-And-Knowledge-Bases)
* Connecting the system with other software and hardware in various ways, and handling multiple-channel I/O simultaneously