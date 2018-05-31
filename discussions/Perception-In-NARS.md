> ###Perception In NARS  
> How to do perception in NARS

***

####Basics

In the context of NARS, "perception" is the process of organizing the system's experience into its internal representation, as concepts, beliefs, and tasks, as well as obtaining additional information about their priority and so on.

Perception is based on sensation, but provides more complicated structures and patterns to satisfy the needs of the system. Perception is restricted by the system's (1) sensitive capability (what can be experienced), (2) perceptive capability (what can be derived from experience), and (3) motivations (what are important and urgent). Since the result of perception depends on the system's experience, perception in NARS is not an attempt to model the world "as it is", nor to emulate human perception in all details.

In NARS there is no separate "perception module" (or modules for vision, hearing, etc.). Perception is unified with other cognitive processes, including reasoning, learning, and so on. Notions like "perception" can be used to stress certain aspects of this process, but not to isolate a sub-process from it.

####Sensation

The early implementations of NARS only depend on a "native input" channel, where the system's experience is a stream of Narsese sentences that can be directly added into the system's memory, which can be considered as a conceptual graph. Beyond that stage, the system can be extended to carry out sensation and low-level perception, so as to get experience consisting of other types of stimulus, and to integrate them into the system's memory.

NARS can have multiple sensory channels, each corresponds to a type of sensor that converts a form of stimulus in the external or internal environment into the system's experience. Such a channel can either be built into the system at compile time, or connected to the system at run time. It is taken to be an organ or tool of the system, to be invoked by by executing certain commands with arguments, as introduced in NAL-8, and producing output values in return.

In general, the difference of the output values can be either _qualitative_ or _quantitative_. 

If the channel C produces values that are qualitatively distinguished from each other, then when the value is _Vi_, the experience can be expressed as "(Ti * Vi) --> C", where _Ti_ is a term representing the current sensation, the channel is expressed as a relational term representing an attribute, with _Vi_ as a value. When there is no ambiguity, the statement can be simplified into "Ti --> [Vi]". For example, "(this-thing * red) --> color-of" can become "this-thing --> [red]". The truth-value of the statement represents the quality or strength of the sensation, determined from the feedback of the sensor and other factors.

If the channel C produces values that are quantitatively distinguished from each other, then to be general the values are assumed to be real numbers. In this case, the channel itself is taken to be a property, and the value is transformed into truth-values of the statement "Ti --> [C]" %f,c%, that is, %f,c% comes from Vi plus other factors, such as the feedback from the sensor on sensation quality and so on. This transformation can be provided to NARS in a problem-specific manner, or use the general semantics of Narsese as introduced in [The Interpretation of Fuzziness](http://www.cis.temple.edu/~pwang/Publication/fuzziness.pdf), which is summarized in the following, using the temperature of a patient as an example.

Assume the system has no initial knowledge about human body temperature at all, and has to categorize patients on this property. By default the system will form two opposite concepts: "high-temperature" and "low-temperature", and the truth-value for a patient to belong to one of them is the negation of the other. The related truth-values are built up gradually by comparing a patient's temperature to that of the other patients, according to the system's experience. For the first patient _p1_ whose temperature is _t1_, since there is nobody to be compared to, the statement "{p1} --> [high-temperature]" will have a confidence 0 and the corresponding frequency 0.5. If the second patient _p2_ comes who has a temperature _t2_, then the frequency of the former statement can becomes 1 (if _t1 > t2_) or 0 (if _t1 < t2_), or remains unchanged (if _t1 = t2_). The confidence of the statement will be _w/(w+k)_, where _w_ is 1 or less, depending on the quality of the evidence. In the long run, the frequency of "{p1} --> [high-temperature]" will be the quantile of _t1_ among all known temperature values.

A more accurate measurement of frequency takes the difference _t1 - ti_ into account, by using its sign to separate positive and negative evidence, and its absolute value as the weight of the evidence.

In summary, all types of sensory values will be "normalized" into values in [0, 1], and eventually become truth-values in Narsese.

####Low-level perception

Low-level perception organizes the sensory inputs into temporal and spatial patterns.

The front-end of each sensory channel is a receptive field, consisting of sensors of the same type in a multi-dimensional space. For simple instances, an auditory channel can have a single sensor, a tactile channel can have a vector of sensors, while a visual channel can have a matrix of sensors. When there are multiple sensors, their relative position (i.e., indexes) provide spatial information.

All the values of the same receptive field are taken as a special type of term, "sensational term", as a snapshot of the sensation. For instance, a two-dimensional receptive field will produce a matrix _Mnnn_, which will get an automatically generated term '_Tnnn_' as name (here '_nnn_' represents a serial number), so the Narsese statement returned by the operation is "_Mnnn <-> Tnnn_". Here '_Tnnn_' is just like the other terms, while '_Mnnn_' conceptually is equivalent to the conjunction of the individual values in the matrix plus the spatial information about their relative location.

Based on this definition, the similarity between these terms can be calculated, according to the extent of overlap of the sensational terms. Inheritance statements can be formed between sensational terms and terms describing the patterns included in the sensation, which in the simplest case will just be a partial and approximate description of the receptive field.

Each sensory channel has a buffer with a constant capacity (corresponding to the 'duration' parameter) that holds what the system senses "at present". On this buffer temporal-spatial term operators are triggered to form compound terms. For instance, multiple sensors that are activated at the same time will form a spatial pattern, such as a line or a blob, while sensors activated in successive time will form a temporal pattern. The components in such a pattern do not have to be continuous in the receptive field, though simpler patterns will have higher priority. For the same reason, compounds whose components are temporally or spatially closer to each other are easier to be generated and kept longer.

**ISSUE**: _How to describe a spatial pattern in a receptive field? Should they to be described with respect to certain coordination system? How to prevent a component from participating in multiple compounds?_

Outside the buffer, the terms involved can be used together with other terms, so as to allow perception to be based on multiple modalities, as well as to be related to the system's existing knowledge. For example, various colors are represented by combining the primary colors, where the involved receptive fields need to be aligned properly.

Though in principle there are many possible sensory patterns that can be formed and recognized, only those that repeatedly appear in experience and contribute to the task processing of the system will be kept, while the others will be gradually forgot. Here the situation is basically the same as in cognition, except that temporal-spatial adjacency also triggers the composition of compound terms.

During perception, attention allocation not only will happen among concepts, but also may happen within a receptive field. For example, the center of the field may get more attention, though its location with respect to the environment can be changed by mental operation (e.g., eye movement).

Different perceptions of the same sensation is possible, though the revision rule and the choice rule will attempt to get the best interpretation of the sensation, and implicitly inhibit the alternatives when possible. In this competition, the related factors include:

1. Evidential support of each perception (i.e., the expectation of the corresponding statement)

1. Simplicity of the description (i.e., the syntactic complexity of the concept of the result)

1. The priority of the concept (quality, usefulness, relevance to the context)

These factors will balance against each other. The competing perceptions may even be at different levels of description. We can expect many phenomena discussed by Gestalt psychology.

####High-level perception

"High-level perception" usually refers to perceptive process at conceptual level, without directly referring to receptive fields, even though the involved concepts do not necessarily have verbal descriptions in a communication language. On NARS, the input to the high-level perception can come either from the low-level perception described above using the system's sensory organs, or from other devices that are blackboxes to NARS that can be invoked by certain operations to get certain information about the environment. Either way, there is no requirement on the nature of the sensors --- they do not need to similar to human sensors, or limited by our current knowledge.

Since a 'term' is NARS is an identifier with experience-grounded meaning, not a 'symbol' waiting to be interpreted to become meaningful, the distinction between perception and cognition is a matter of degree, roughly corresponding to the distance from a term to the sensory terms. It is quite common for an "abstract concept" to have perceptual aspects, as well as for a "mental image" to have conceptual aspects.

As the system's experience constantly unfolds in time, the meaning of a term is never fixed, but change over time. Furthermore, when a term is used, only part of its relations are involved, due to the resource restriction and attentional focus of the system. Even so, in a given time some terms may have relatively stable meaning. Sometimes it is possible to identify certain relations about a term (or concept) as its "essence" or "definition", in the sense that they are stable and can derive the other relations. However, this is usually an approximation, and even such an approximation is not always possible. Many terms (concepts) cannot be defined at all, though they are still meaningful. It is just that their meaning cannot be briefly and reliably summarized. Terms corresponding to words in natural languages may have "conventional definitions" made by the community using the language, but these definitions do not fully capture the meaning of these terms to the system, though contribute to it. Actually speaking, a term in NARS has no "correct" or "true" meaning, nor does it converge to such a meaning in the long run.

Object and event recognition usually requires multiple levels of abstraction and composition, though the terms do not form an accurately layered structure, but a conceptual graph where links can go in any direction, and between any pairs of concepts (in principle). The experience-grounded semantics still applies to perception, though some concepts have fixed aspects in their meaning (as for operations).

Beside atomic terms that are simply strings in an alphabet, NARS has various types of compound term, formed by a logical operator (or connector) and one or more component terms, which can be compound terms themselves. There is no limit in the level of composition, though terms with complicated internal structure are not favored in resource competition, other factors being the same.

This mechanism let the system compose new concepts from existing ones. When the terms involved are operations, this is how skills and programs are formed recursively. Then the terms correspond to sensory signals, the compound terms correspond to patterns formed by these signals recursively. In principle, all meaningful concepts can be formed in this way.

NARS does not search (deterministically or randomly) the space of all possible compound terms. Instead, new compound terms are formed only when the corresponding structure happens in the system's experience. For example, the term (&, [red], apple) (for "red apple") is formed only when the system knows an apple that is red, or when the system needs to find an apple that is red. The system may never form the term (&,[blue], apple), though it is understandable. Unlike proposed by some researchers, NARS does not create new concepts by arbitrarily "blend" or "twist" existing concepts.

Furthermore, the system does not keep a concept for every compound term it ever formed in history. On the contrary, most of them are forgot, and the ones kept are the concepts that have relatively high priorities, which means they have a relatively rich and stable meaning, as well as have been useful in the past in processing the tasks. In this way, the evaluation criteria for what concepts to keep are not coded in a fixed formula or algorithm, but are gradual, cumulative, and statistical. For a pattern in the experience to become a concept, both its occurrence frequency and its usefulness to the system matter. Also, if the pattern is complicated, it may take a long time for the system to recognize it, if it is found at all.

####Conceptual Relations

To specify "meaning" by "relations" or "usages" is not really a new idea in AI and cognitive science. For example, similar ideas can be found in semantic networks, neural networks, and statistical semantics. What makes NARS different from the other approaches is how these "relations" are represented and processed.

NARS recognizes the following types of relations among terms:

* Syntactic relations between a compound term and its components. This types of relation is identified by the operator plus the index of the component in the compound. These relations are binary --- there are either there or not, though the syntactic complexity of the compound term may be considered.

* Semantic relations between terms. There are only four of them: inheritance and similarity indicates how the extension and intension of two terms are related; implication and equivalence indicates how the truth-values of two statements (as special compound terms) are related. These relations are measured by the two-factor truth-value of NAL.

* Temporal-spatial relations between terms. They are not used alone, but are embedded in the other relations or mechanisms. The temporal relations are based on the occurring order of events, as defined in NAL-7. The spatial relations are implied in the spatial arrangement of the related sensors. For example, if the system has a group of light sensors arranged in a matrix, then their relative position will represent spatial relations among the corresponding terms.

The above types of conceptual relations are built-in, in the sense that they are directly recognized and processed by NARS, and therefore with fixed meaning. They can be described using "meta-terms". On the contrary, all the other conceptual relations are acquired as terms, with variable meaning. For example, the "parent" relation between "Tom" and "Mary" is actually represented and processed in NARS as an inheritance relation between compound term "(x, {Tom}, {Mary})" and a relational term "parent", as defined in NAL-4.

For a given concept, all of its relations with other concepts collectively determines its meaning, though at a certain time some relations may contribute more than the others. However, the meaning of a term or concept is rarely determined by a single relation. The meaning of a compound term cannot be reduced to its components via the syntactic relations. If a term names a sensation, a perception, or an operation, such a relation still do not decides its full meaning.

NARS concepts are naturally multi-modal. For example, the meaning of "cat" may consist of the vision of cats, the sounds they make, their smell and touch, the system's knowledge and beliefs about them, as well as the pending questions and goals about them.

####Categorization Process

In this context, "categorization" is the process by which a concept is created, modified, evaluated, and used in various ways. In NARS, this process is fully carried out by the inference rules, so it is the same process as "reasoning" and "learning", though each notion stress certain aspects of the process.

The concepts in NARS is not generated by a single algorithm, but by various rules in different ways, triggered by suitable experience of the system. Similarly, a certain conceptual relation can be built in more than one ways, which means the same question about categorical relationship can be answered in different ways.

With inference rules processing the built-in relations, NARS does not merely use conceptual relations to express meanings of concepts, but can also effectively manipulate these relations. This cannot be done in a semantic network, where a link can represent any relation, so there is no clear way to use it; this also cannot be done in a neural network, where a link merely means two nodes are related, with little further information.

A question on categorical relationship typically requests the evaluation of a semantic relation, and the answer is usually a matter of degree. Such a conclusion is usually the result of a cooperation of multiple inference rules in an inference process. Beside the available knowledge, this process is also influenced by the available resources. This process is history-dependent and context-sensitive, and usually not accurately repeatable.

####Compared with other AI Techniques

The approach NARS taking for perception is fundamentally different from the most existing approaches.

* NARS does not take perception as a separate module or process, but see it as unified with other cognitive processes.

* NARS does not take perception as a purely input-driven, "data-mining", process that passively accepts all sensory data as the input at the beginning, and produces certain desired results at the end. Instead, perception is taken to be an interactive, incremental, lifelong, and open-ended process that is driven both by input data and active tasks (goals).

* NARS does not following a predetermined algorithm at the problem-solving level. Since many factors are ever-changing and their combinations are not repeatable, the process does not follow an overall algorithm. Even for the same input, in different context the results of perception may be different, though they are neither arbitrary nor random.

The development of perception in NARS will be more like what happens in a baby than in an traditional AI program.

####Implementation and Application

To realize perception in the OpenNARS implementation, the following steps will be taken.

1. Identify a proper testing domain. The system should be able to get continues input, probably both in some Narsese channels and some non-Narsese channels together. Natural language channels are possible, too, but maybe at a later stage.

2. Implement the necessary sensors and actuators as NARS operations. NARS allows multiple levels of granularity of a certain modality. Take vision for instance, it is fine to have operations corresponding to the sensation of pixels, as well as more complicated operations that directly recognize objects of certain type, or a mixture of them.

3. Confirm that Narsese has enough expressive power for the perception-related compound terms, especially various temporal-spatial patterns. If necessary, revise or extend the grammar rules.

4. Confirm that NAL has enough inferential power for the perception-related tasks. It may be necessary to implement certain special-purpose "macro-rules" for important perceptual routines.

5. Identify and implement the mental operators needed for perception-related tasks.

6. Refine the control mechanisms to efficiently carry out perceptual processes, as well as to balance them with other processes in the system.

7. Explore various perceptual tasks. Beside the traditional tasks like the learning, recognition, and clustering of perceptual patterns, there are also tasks demanding sensorimotor coordination and perception–cognition continuum, like imitation, skill learning from reading, etc.