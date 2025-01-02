from the book [Residues](https://leanpub.com/residuality)

## What is Architecture

**Architecture** is the structure of a software application that **drives the behavior of a software application in its environment**. That is, the way the system responds to scale, load, changes, integrations, and things we normally refer to as non-functional concerns.

**Functionality** is the concern of the developer and only **concerns what exactly the software should do**. This **functionality should be able to run on the architecture**. The **architecture’s job is to make sure that it continues to run as the business environment changes**. Architecture is as such nothing more than advanced programming or software engineering–a level above data structures and algorithms.

## Time, Uncertainty, and Change

Traditional methods of architecture focus on producing static pictures–architectures captured as a set of components and their relations. These traditional approaches suffer from three very serious issues. They have no way of capturing time, uncertainty, or change.

- Software are execute to support a business environment.
- The business environment is subject to changes due to influence of customers, markets, competitors, economies, social structures.
- This changes are not predictable and mappable.
- Architecture is subject to evolve in a way that is not predictable by the architect who design the system
- Software require certainty in order to be written

This is why architecture is difficult
When architecture fails, it is due to unexpected changes in the perception of the business environment. 
The blame of `poor or incomplete business requirements` is a symptom more than a cause, because the **business requirements actually are not predictable**.

## Unit of Software Architecture

The residue is the unit of measure of software architecture.
It is what remains of a system after it is exposed to some form of stress.

## Ergodicity and Hyperliminality

**Ergodicity** is a property of a process to sum-up the behavior of a more complex one that is you can predict future states of the system knowing the past behavior.
- Human system are not ergodic
- Software is ergodic
In other terms an:
- ergodic system are complicated
- non-ergodic system are complex

**Hyperliminal** system is a complicated, ergodic, ordered system executes in a complex, non-ergodic, disordered context

**The job of an architect is to design an hyperliminal structure for the ergodic system that survives the changes in its non-ergodic context.**

Software Engineer has imported practices from other engineering that work instead in environment where the context is more stable.
Instead of learning to deal with an environment in constant flux we enforced in prediction and control in order to reach the level of other engineer practices
Instead of working toward a better possible solution, software architecture became an exercise in control, prediction, and imitation.

>Software Engineering is the engineering of hyperliminal systems. There is nothing in the history of engineering that is like this, and no grounds for importing historical concerns. The history of software engineering thus far has been tantamount to nailing horseshoes to car tires.


# Residuality

## What is Residuality?

>A residue is what is left over when the software system is stressed. The residue becomes the base unit for describing a software architecture.

>Residuality is a philosophical idea that allows software architects to engage with non-ergodic, hyperliminal systems in an honest way.

>Residuality is a philosophical idea that allows software architects to engage with non-ergodic, hyperliminal systems in an honest way.

When you think of architecture you will no longer think of static diagram, enriched with some risks and probabilities but you will be able to visualize a sequence of uncertain architectures over time in a fast-changing environment.



