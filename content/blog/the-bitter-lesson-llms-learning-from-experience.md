---
title: "The Bitter Lesson, LLMs, and Learning From Experience"
date: 2026-09-27
description: "How pretrained knowledge and continual learning from experience might complement each other in future AI systems."
summary: "LLMs inherit knowledge from human-generated data, while reinforcement learning helps AI learn from action and feedback. The future may combine both."
tags: ["AI", "LLM", "Reinforcement Learning", "Continual Learning"]
categories: ["AI"]
author: Nimendra
ShowReadingTime: true
ShowPostNavLinks: true
ShowBreadCrumbs: true
draft: false
---

I recently read Richard Sutton's [_The Bitter Lesson_](http://www.incompleteideas.net/IncIdeas/BitterLesson.html) and went down a small rabbit hole. [Dwarkesh Patel's conversation with Sutton](https://www.dwarkesh.com/p/richard-sutton) sent me even further down it.

I kept coming back to the relationship between **pretraining and learning from experience**.

LLMs learn from huge amounts of human or machine generated data. They are next-token predictors. I think the current AI hype sometimes creates the illusion that LLMs learn the way humans do while answering us. Much of what looks like learning comes from the amount of data used during training and the patterns the models capture.

Sutton emphasizes a different direction, AI systems that learn continually through **experience, action, and feedback from the world**.[^rl]

That got me thinking about how humans learn. _We learn through our own interaction with the world, but we also learn through knowledge passed down by previous generations._

> For example, a tribe living in the Amazon jungle may know which plants are poisonous or which snakes are dangerous because earlier generations discovered and preserved that knowledge. Learning all of that again through trial and error would be inefficient, and sometimes dangerous.

{{< figure src="/images/bitter-lesson-human-learning.webp" alt="Comic contrasting learning from personal interaction and trial and error with knowledge passed down by earlier generations." caption="Humans learn through experience and knowledge passed down by earlier generations." width="400" align="center" >}}

I see a similar possibility for AI.

What if we build systems where **reinforcement learning and continual learning are the main learning process**, while pretrained knowledge is used as accessible prior knowledge?

Humans and cultures build on what earlier generations learned. We do not need to rediscover everything from scratch. We learn some things through direct experience and feedback, and other things from people who came before us.

Pretraining could provide inherited knowledge. RL and continual learning could let a system test and correct that knowledge through interaction with the world.

[AI agents](https://notes.nimendra.online/00.fleeting-notes/ai-llm/ai-agents/1.-introduction-to-ai-agents#what-is-an-ai-agent) move toward this approach by letting models interact with environments and tools. Techniques such as RLVR use feedback to shape models during post-training.[^rl]

_But fundamentally, the underlying LLM is still a pretrained model. It is not yet a truly continual-learning system that keeps updating itself from everyday experience._

The future may combine pretrained knowledge with continual learning from experience, rather than choosing between LLMs and RL.

My notes from this _Bitter Lesson_ rabbit hole are here: [The Bitter Lesson and Richard Sutton](https://notes.nimendra.online/00.fleeting-notes/ai-llm/papers/the-bitter-lesson-and-richard-sutton)

{{< figure src="https://notes.nimendra.online/00.fleeting-notes/ai-llm/papers/the-bitter-lesson-and-richard-sutton-og-image.webp" alt="Preview card for the Bitter Lesson note on Nimendra's Notes." width="500" height="auto" align="center" link="https://notes.nimendra.online/00.fleeting-notes/ai-llm/papers/the-bitter-lesson-and-richard-sutton" >}}

[^rl]: In current LLM discussions, **RL** often refers to post-training methods such as **RLVR (Reinforcement Learning with Verifiable Rewards)**. RLVR is a form of reinforcement learning, usually applied to bounded, human-designed tasks with verifiable reward signals. Here, I mean the broader idea of **learning from experience**. An agent interacts with its environment and observes the results of its actions. It then updates its knowledge and behavior. RLVR is one form of RL, but it is not the open-ended continual learning discussed here.
