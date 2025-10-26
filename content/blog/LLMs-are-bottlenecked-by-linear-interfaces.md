+++
title = "LLMs Are Bottlenecked by Linear Interfaces"
date = "2025-10-26T09:37:27+01:00"
draft = false

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["ai agents","generative ai","software engineering","stream of consciousness"]
+++

## Current limitation as I see it
So I have been experimenting quite a bit with LLMs recently (see my other posts,
especially [/state-of-ai-assisted-workflows-october-2025/](/state-of-ai-assisted-workflows-october-2025/)) and dedicated some
serious amount of effort to play around with the tech to both learn more but
also to build an intuition for what it is and what it can do well etc. This has
led me to having a realisation or an idea, that I'm sure is not unique but I have not seen much
about it being mentioned yet. The idea is that our current LLMs are being
bottlenecked, leaving performance on the table, by the interfaces we have when
using them. Mostly our interactions with these models are through a chat
interface and that forces linearity which also makes context pollution the huge
problem it currently is.

An adjacent contributor to this problem is the fact that the "tests" or
benchmarks that are used to evaluate these models reward guessing when not knowing.
It's like most tests in school, if you are wrong you don't lose points, but if you
guess and it's the correct answer you get the reward. In such an environment we
are selecting for models that guess when uncertain. Now if that is good or bad,
I can see how one can argue for both, but it's important to recognize this I
think. Because all the current solutions to deal with this problem are in some
sense also trying to impose guardrails to make a non-deterministic system
behave deterministically.

## Current problem with existing solutions
I view most of the current solutions such as swarms of subagents, Spec-kit,
openspec and other SDD frameworks and .md files such as cursor rules as band-aid
solutions to the underlying problem.

These approaches are essentially trying to do context management. Their whole idea
is to get the LLM to deterministically output something that follows some set of
rules and preferably those rules should be shareable to replicate the state of
the model that best aligns with whatever it is that is trying to be achieved. 

But they are not able to fulfill that promise consistently in most cases. I
think it's because the tools given, i.e. just using text and natural language,
are not powerful enough.

## The idea proposal for a possible solution
We need to move away from having these linear interfaces where we have prompt ->
LLM output -> prompt etc, then for a new feature or slightly different topic we
create a new session starting from scratch and again the linear chain.

Instead, if we think of each state as a node in a tree structure then we can
branch off from current state, do some things, evaluate and only bring it back to
the main branch if deemed useful by a human. This means that we should not think
in terms of sharing a bunch of text files but rather be able to share the entire
state.

Simply put this means that instead of having to trust that the agent does read
all the .md files and follows all the Skills and loads them into the context
correctly each new session, I want to be able to share the session / state
itself. 

This would enable a team to create a number of states for each type of task and
then run proper evals on top to determine what state is best suited for the type
of tasks at hand, and if any changes to that state makes it better or worse.

## Clarification of what I'm after
So what I'm proposing is not an agent system prompt or just something that we can
get by pasting in a context.md file etc. I want to be able to prime the agent
into those rare moments where the agent feels like it's an extension of you and
is totally understanding what you are saying without too much prompting, those
moments that have gotten us all hooked and wanting more, I want to be able to
save that state and be able to re-use it for other tasks without having to do
anything other than "navigate" to that state in the UI and start prompting. 

The one tool that is something close to what I imagine currently would be [beads](https://github.com/steveyegge/beads)
but that is still relying heavily on these txt files, because that's all we got
currently.

