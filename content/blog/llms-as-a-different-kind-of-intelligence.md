+++
date = '2026-06-14T11:01:18+02:00'
draft = false
title = 'LLMs as a Different Kind of Intelligence'
tags = ["ai agents","generative ai","software engineering","stream of consciousness"]
+++
As I was writing this post I could not crystallize who the target audience for
it is, other than for myself to put into writing how I view and think about
LLMs. I have spent enough time with these systems by now that I have stopped
trying to map them onto things I already understand, because every analogy I
reach for ends up breaking somewhere. The most honest thing I can say after all
this time is that what we are dealing with is a different kind of intelligence,
strictly different from our own, and I think most of the confusion around these
tools comes from us pretending otherwise.

Even the words we use give the game away. The term agent is overloaded for sure.
An agent is something that works by itself based on some type of direction. Yes
this exists today, but most of the existing workflows are also more like
scheduled jobs that run with some kind of dynamic properties that was not
possible before. It is the same with words like cognition and thinking. Using
those words when talking about genai feels inaccurate to me, but I will go with
them for the sake of argument because we do not really have better words yet.
That is kind of the whole problem, we keep borrowing human vocabulary for
something that is not human. Models as they are today do not think as we think,
and using those words leads to expectations and assumptions that are simply not
true.

This is why I think the comparison that is often made, where LLMs are compared
to junior engineers, is flawed, simplistic and a false narrative designed to
sell. The reason is that I do not subscribe to the idea that we can compare
human and LLM cognition at all. They are not a worse version of us that will
eventually grow into a better one. They are something else.

Yes, in the beginning a junior engineer might not produce solutions as fast, or
even as well, as an LLM does today, but a junior can connect dots and understand
the intent behind the proposed fix or why the problem was encountered to begin
with. A junior engineer can also use their own unique ideas and even fully
ignore the assumptions and premise of a task because they realize what the user
wants or how the problem even comes about, which is that intent understanding
that an LLM simply does not have today.

The other analogy I was fully onboard with until a while ago is the idea of
LLMs being a new abstraction layer. I think that idea is directionally correct
but not fully. Usually the comparison is that we have gone from writing machine
code, to compilers, to higher level languages, and now we are at the next step
which is LLMs writing the code. However I do not really think this is accurate
anymore, because each of the previous abstraction layers can be peeled back if
needed, and even more importantly each step through those layers that the
machine takes for us is deterministic and can be replicated and fully
understood. How the LLM comes to its conclusions, how it decides on some
implementation, how that is affected by each word in the input, and then on top
of all that a certain level of randomness, none of it fits the pattern of the
previous generations of abstractions it gets compared to.

If you zoom out a bit there is actually an interesting shift hiding in here. For
most of computing history the layers underneath us were deterministic, and the
probabilistic stuff lived off to the side in data science and machine learning.
With agents that probabilistic behavior has come all the way up to the surface
and is now the thing we work with directly. So the deterministic mental model I
have always used for programming and the probabilistic mental model I used for
data work are slowly converging into one, and honestly that is a bit
uncomfortable, because the whole discipline of software engineering was built on
top of determinism.

So if it is not a junior engineer, and it is not just the next abstraction
layer, what is it? Honestly the most accurate description I have landed on is
almost a contradiction. It is a system that knows everything and understands
nothing.

It has read more than any human ever will, and yet it cannot connect the dots on
its own, not even at the level of a child. A child at least asks why. The model
just does. I notice this constantly. I can ask it to write a pipeline and it
will produce something that runs, uses the right libraries, even handles edge
cases I did not mention. But the moment the actual goal shifts slightly from the
words I used, it keeps optimizing for what I said instead of what I wanted,
because it never had a model of why I wanted it. It knows the syntax, the
algorithms, the data structures, all of it. What it does not have is intent.
That combination does not exist anywhere in human experience, which is exactly
why our analogies keep failing.

You could argue that with enough context the model would not make that mistake,
and for a single well bounded task you might be right. But intent is not the
same thing as context. Context is the information you can put in front of it,
the files, the ticket, the constraints. Intent is the why underneath all of
that, the thing a person keeps measuring every small decision against without
even thinking about it. The real tell shows up over time, not in any single
task. The model does not fail loudly, it drifts. Every individual decision looks
reasonable on its own, and yet they slowly accumulate into something that has
quietly wandered away from what you actually wanted. Leaving them alone to work
on something for too long always leads to a loss of alignment with the vision
and intent of the user or engineer, in a way that a human who knows everything,
or is smart enough and experienced enough to be as technically capable as LLMs
are at engineering, would never. We do not have a reference point for an
intelligence like this.

And I think that missing reference point is where most of the problems and the
wildly differing opinions come from. It is what leads to the discussion of how
some people are not even opening their IDEs anymore, while others cannot get
models to do anything useful for them. Think about what you are implying when
you treat them as just some advanced autocomplete that has to be told what to do
step by step. You are saying that on a mechanical level the system already knows
everything it needs. It just cannot capture intent, so it needs you to spell
everything out.

None of this is to say analogies are useless. The mistake is forgetting that
they are analogies. The junior engineer framing and the abstraction layer
framing are both reaching for something true, they just stop short and then get
sold as the whole picture. I think we would all get more out of these tools if
we stopped trying to force them into shapes we already understand, and instead
treated this as its own kind of intelligence with its own failure modes, and
then went and learned those failure modes directly. It is not a smarter version
of us, and it is not the next compiler. It is something genuinely new, and the
sooner I stopped pretending I already had a box to put it in, the more useful it
actually became.
