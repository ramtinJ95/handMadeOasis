+++
title = "Agent at Home Part 1a"
date = "2025-11-30T09:54:01+01:00"
draft = false
#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["ai agents","generative ai","software engineering", "developer tooling",
"agent at home series"]
+++

Before we get into the coding part of the series I think it would be beneficial
to give some background to what the ReAct loop is that we are going to make use
of to build this agent and also describing a bit how the code is going to look
like. This will not be that long to be fair because the basic loop is fairly
simple once you grok the ReAct loop.

Note: The goal of this series is not to explain how LLMs work under the hood,
no deep dives into transformers, attention mechanisms, or training processes.
While I personally find that stuff fascinating and have spent time learning it,
this series is about engineering. And here's the thing about engineering: you
don't need to understand every layer of abstraction below you to build something
useful on top of it. I don't need to understand semiconductor physics to write
good software. Similarly, you don't need to understand backpropagation to build
a capable agent. Scientists discover and seek understanding as the end goal.
Engineers build, and understanding serves that goal. Both are valuable, they're
just different. This series is firmly in the engineering camp.

## What is an agent
Let's start with me defining what I think an agent is: An agent is essentially a
while loop that leverages an AI model (i.e Claude, GPT, Gemini etc) to interact
with and reason about its environment in order to accomplish a user defined goal(the
user prompt basically), those interactions are mediated through external tools.

Now it is sometimes useful to categorize agents on a spectrum of different
agency levels. On such a spectrum this definition of an agent is fairly high up
on the agency level and would be called something like a multi-step agent.
Since the output controls the program flow. The goal of this series is to start
with a multi-step agent and move towards fully multi agent which means an agent
that can start other agentic workflows.

I mentioned the spectrum because there are other simpler agents that can also be
very useful such as a router agent which with its output determines some basic
control flows, i.e which path code execution should go through based on some
decision that would be hard to code for in a deterministic way. But that is not
the focus of this series.


## ReAct (Reasoning + Acting)
The core ReAct loop is actually very simple conceptually. All that is
happening is: Thought → Action → Observation → (repeat)

This means that you send some prompt and that prompt includes your system
prompt, what tools are available and what the objective is. The LLM then reasons
about the task and then figures out it needs to use one of the tools in the tool
list you sent in your request. So it sends that back in its response, the agent
then executes that tool, this is the action step. The output of that action is
sent back to the LLM with the entire message history (this is the context) and
the LLM again reasons about the next step. This then repeats until the final
response from the LLM does not require any more action, which means it either
has an answer or needs more input from the user. That's all there is to it.
For more details read the [ReAct Paper](https://arxiv.org/abs/2210.03629).

I think the reasoning part is worth going into a bit deeper though because there
is some terminology and methods that interact in that step that I initially
found a bit confusing.

### Chain of thought's role in the reasoning step
Chain of thought (CoT) was all the rage like a year ago, maybe less. That is an
eternity in AI so I think it's worth explaining CoT's role in the ReAct loop.

Basically CoT is a prompting technique where us as users nudged the models to
produce intermediate reasoning steps before coming up with the answer, or at
least show the intermediate steps, and this improved the accuracy on reasoning
tasks. 

So this means that the ReAct loop is using CoT in its thought/reason step as
explained earlier above. The confusing part is what is shown to the user and
what the model is doing implicitly so let's dig a bit deeper still.

State of the art AI models are, and have been for a while, trained
(fine-tuned after initial model training to be precise) to generate these
internal chains of thought by default and then distill them into a final answer.

The word internal is doing a lot of heavy lifting there. Essentially all the
"thinking" AI models are doing internal/hidden CoT steps even if you as a user
don't ask for it. This hidden reasoning is done with "reasoning tokens" and uses
what the model providers call a scratchpad. The external or reasoning steps that
are shown to us as users and can be returned in a ReAct loop for our agent is a
cleaned up and summarized version of that internal reasoning. This means that
the role of CoT has changed from a "hack" to get better output out of the AI
models to more of a UX way of getting the reasoning steps in a format that you
need or require for debugging/understanding why the agent did what it did. What
this means practically is that when you enable extended thinking etc for these
models the budget for those reasoning tokens is what you are increasing, it's not
that you are enabling a new behaviour that they did not have previously.

Just to be clear though, for weaker models that do not have this internal CoT
process baked in, CoT as a prompting technique to get more performance out of
the LLM at the cost of higher context usage could still be viable.

## Rounding off
Alright I think that is all the conceptual knowledge that is required to
understand why the code we are going to write is looking the way it's going to
look. As you can see the concept is at its core very simple but building it in a
useful way to extract as much performance as possible out of the AI model is the
fun engineering challenge we are going to embark on now!

---

**Next up:** [Agent at Home Part 1b](/agent-at-home-part-1b/) , where we dive into the actual code and build the agent step by step.
