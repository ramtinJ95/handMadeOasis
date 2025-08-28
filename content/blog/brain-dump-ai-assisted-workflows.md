+++
title = "Brain Dump: AI Assisted Workflows"
date = "2025-08-28T23:15:26+02:00"
draft = false

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["developer-tooling","personal",]
+++

This is just like a note or dump of everything I research on AI agents, MCP, and
other generative AI stuff. I think the correct phrase for it is a stream of
consciousness dump

There seems to be a kind of tier or ladder of sophistication that is slowly
getting shaped when it comes to AI assisted workflows, especially in relation to
software engineering. As I see it currently we have the use of online chat
interfaces, this is where you have chatGPT, Claude and company. At this point 3 years in,
this I think we have to admit has become table stakes pretty much. The next
level up is having an agent side by side your daily work. Here you have direct
access to the LLM and if you use a vscode fork like Cursor or any of their
competitors its usually just another pane on the right side of your code. This
is pretty nice, especially when you start to use it. It just feels so nice to
not have to leave your editor (personally i hated having to use these vs code
forks, and so glad the next level now exists), and just have the LLM have some
context around your code and so on. 

Now the third level, which I'm currently on as of 2 weeks (28/08/2025) is to use
a proper coding agent through the CLI. For me this was a super welcomed and
natural transition actually, since I love the terminal and I really wanted to
get the hell off of vscode and back to my beloved Neovim. The premier tool for
this level is currently Claude Code, but I actually use opencode which is the
open source version of Claude Code. Now the capabilities of the tool and
enjoyment of using a LLM is finally starting to really impress me. I guess that
is the reason that i now have spent a significant time the past 2 weeks
researching this stuff, its been out for a while but I have just not thought it
would be worth my time, how wrong I was. These coding agents unlock true
productivity gains, and not only that but they also make the use of LLM's
finally feel like engineering. 

The reason I say that it finally feels like proper engineering is because at
this tier/ ladder step/ level one has to put in some proper effort and systems
to get the really awesome results. For these agents you can have custom commands
which are pretty much prompts that can be used based on situation, these can then
be used by the entire team. You can have project specific and/or global prompts,
this then means that technically one could run evals on these, or save the chats
and outcomes then once changes are made to these team wide prompts we can see if
performance drops on tasks that are relevant to the team context and workflows.
These tools even allow for creation of custom agents and subagents which can be
spun up and used by a primary agent. Now this type of workflow is the next tier
and I'm not there yet, its a bit over my head currently to start having swarms of
these agents and so on. 

I have not even mentioned the hyped MCP stuff that makes these coding agents
even more powerful, where now we can start to really combine the different
actions that the agent can take in our environment. But you say, correctly,
all of these things were also possible with Cursor and copilot in vscode.
Somehow that workflow never stuck with me when I was using Cursor. And with
Cursor, since it's an IDE pretty much you always have to guide it and keep an eye
on it. While I feel like these coding agents can run off do their thing and I
can focus on something else while they are doing what I told them, there is no
cognitive load there. This essentially means that when I open my editor today, I
want to write the code (although i do use copilot autosuggestions when they make
sense) and if i need to ask something or have the AI assist me in some way I
will change to a different tmux tab that i have running and ask or tell it to do
something while I stay with the context I had and keep working. 

**All of this to say that my current (2025-08-28) AI assisted workflow is:**
- For each project create an AGENTS.md file which gives context about the current
project/repo. This is something that the agents can even generate themselves but
its best to iterate on it for better results. Even better is to iterate on it
together with the AI.
- I have custom commands that I will add as part of my dotfiles. These are
commands for tasks like review, possible improvements and classification of
tasks.
- Write what I'm trying to do or what the goal is and the steps to get there in a
requirements.md file that i have the agent read with the classification
command so that the agent can create a plan and list of tasks etc which I can
review before i send it off.

**Workflow changes that I want to or am evaluating:**
- Somehow save more context of changes made to the repo.
- Incorporate proper eval to see if we are actually improving results when we play
around with all these .md files



### Recreational programming
This will be another post entirely in the future but for me, turning off all
the AI assistance when learning something new or working on projects is very
important. I have found that the less I use AI for my personal projects where
the aim is just to have fun and learn new things, like graphics programming or
writing a programming language etc the more fun I have.

This concept of Recreational programming was something I stumbled upon by the
streamer and content creator tsoding. 


## Tools
Current tools that I'm using or looking into for this stuff:
- FastMCP
- Pydantic-ai
- Opencode.ai
- GitHub Copilot as the access point to different LLM's
- Copilot in nvim for autosuggestions
