# Using Tree-of-Thought prompting to boost ChatGPT's reasoning

## Abstract

Tree-of-Thought Prompting is a novel technique, taking ideas from the Tree-of-Thoughts framework. It expands and improves on the established Chain-of-Thought prompt concept, allowing some Large Language Models such as ChatGPT to show more advanced reasoning. Tree-of-Thought Prompting allows Large Language Models to self-correct errors, whilst gradually building up knowledge.

## Difficult questions for LLMs

There's certain types of questions that Large Language Models (LLMs) find difficult to answer. Here's an example:

    Bob is in the living room.
    He walks to the kitchen, carrying a cup.
    He puts a ball in the cup and carries the cup to the bedroom.
    He turns the cup upside down, then walks to the garden.
    He puts the cup down in the garden, then walks to the garage.
    Where is the ball?

The correct answer is that it's in the bedroom. LLM's are non-deterministic but here's the responses I got with a few tries:

* ChatGPT 3.5: The ball is in the garden. ❌
* ChatGPT 4: The ball is in the bedroom. [followed by an explanation] ✅

## Chain-of-Thought prompting

There's a well known concept called "Chain-of-Thought prompting" or CoT for short. The idea is that you get the LLM to explain its thinking, then it's more likely to get a correct answer. This concept seems to date back to this paper from way back in Jan 2022: [Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/abs/2201.11903).

In essence, you can do this by adding something like this to a prompt:

    Think carefully and logically, explaining your answer.

This helps with many questions but isn't always very effective. Here's an example of ChatGPT 3.5's response:

    Based on the given sequence of events, we can logically determine the location of the ball. Let's analyze each step:
    1. Bob is in the living room.
     - No mention of the ball in the living room.
    2. Bob walks to the kitchen, carrying a cup.
     - No mention of the ball in the kitchen yet.
    3. Bob puts a ball in the cup.
     - The ball is now inside the cup.
    4. Bob carries the cup to the bedroom.
     - The ball is still inside the cup.
    5. Bob turns the cup upside down.
     - If the ball was inside the cup, it would have fallen out when Bob turned it upside down.
    6. Bob walks to the garden.
     - Bob left the cup in the garden, but there is no mention of the ball being removed from the cup.
    7. Bob puts the cup down in the garden.
     - The ball, if it was in the cup, would still be in the cup in the garden.
    8. Bob walks to the garage.
     - No mention of the ball being taken to the garage.
    Based on the given information, we can conclude that the ball is still inside the cup in the garden.

* ChatGPT 3.5 CoT: The ball is in the garden. ❌

You can see how ChatGPT realised the ball fell out at step 5 but still didn't get the answer correct.

Can we do any better, without resorting to ChatGPT 4?

## Tree-of-Thought Framework

There's been a few papers and Python repos the past couple of weeks introducing the concept of Tree-of-Thoughts (ToT):

* [Large Language Model Guided Tree-of-Thought](https://arxiv.org/abs/2305.08291), 15 May 2023. [Github](https://github.com/jieyilong/tree-of-thought-puzzle-solver).
* [Tree of Thoughts: Deliberate Problem Solving with Large Language Models](https://arxiv.org/abs/2305.10601), 17 May 2023. [Github](https://github.com/ysymyth/tree-of-thought-llm) (no code as of 22nd May)
* [Tree of Thoughts](https://github.com/kyegomez/tree-of-thoughts) Github, 21st May 2023

The basic idea is that the LLM is given the opportunity to explore multiple branches of reasoning and then self evaluating as it prorgresses.

From the papers, this looks to be very effective for certain problems.

## Tree-of-Thought Prompting

The existing papers on the Tree-of-Thought framework all seem to be based on making multiple calls to an LLM, (eg using Python). The great thing about Chain-of-Thought reasoning is that you can do it with ChatGPT in a single sentence prompt.

That got me thinking: can we get an improvement in results over CoT by using a ToT style prompt?

Here's the prompt I used:

    Imagine three different experts are answering this question. All experts will write down 1 step of their thinking, then share it with the group. Then all experts will go on to the next step, etc. If any expert realises they're wrong at any point then they leave. The question is...

And here's the response from ChatGTP 3.5:

    Expert 1: The ball is in the living room.

    Expert 2: The ball is in the kitchen.

    Expert 3: The ball is in the bedroom.

    Expert 1: Bob carries the cup to the bedroom, so the ball must be in the cup.

    Expert 2: Oh, I see my mistake. Yes, the ball is in the cup.

    Expert 3: Agreed, the ball is in the cup in the bedroom.

    Expert 1: Next, Bob turns the cup upside down in the bedroom.

    Expert 2: After that, Bob walks to the garden and puts the cup down.

    Expert 3: Therefore, the ball must have fallen out of the cup when Bob turned it upside down in the bedroom. So, the ball is in the bedroom, not in the cup anymore.

    Expert 1: Oh, you're right. I made a mistake. The ball is in the bedroom, not in the cup.

    Expert 2: Agreed, the ball is in the bedroom.

    Expert 3: Bob then walks to the garage, so the ball remains in the bedroom. It is not in the garden or the garage.

    Expert 1: Absolutely, the ball is still in the bedroom.

    Expert 2: Yes, the ball hasn't moved from the bedroom.

    Expert 3: Therefore, the ball is in the bedroom, not in the garden or the garage.

    All three experts agree that the ball is in the bedroom.

* ChatGPT 3.5 ToT: The ball is in the bedroom. ✅

You can see how this time ChatGPT 3.5 gets the answer correct!

# Caveats

* This has not been extensively tested but it does seem to perform better that plain CoT prompts
* The ToT framework includes more than is represented in my example prompt

# Improvements, feedback and contributions

The prompt can likely be improved to make it perform better, potentially giving more concise responses too.

If you have success with any other prompts then please submit a PR and add them to `tree-of-thought-prompts.txt`

# Other thoughts

The Tree-of-Thought framework is similar to an **organisational decision making process**, involving multiple parties.

High performing teams can make better decisions than individuals. It's very possible that other organisational structures and attributes can be used to improve the performance of LLMs.

# Acknowledgements

* [Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/abs/2201.11903), Jan 2022.
* [Large Language Model Guided Tree-of-Thought](https://arxiv.org/abs/2305.08291), 15 May 2023. [Github](https://github.com/jieyilong/tree-of-thought-puzzle-solver).
* [Tree of Thoughts: Deliberate Problem Solving with Large Language Models](https://arxiv.org/abs/2305.10601), 17 May 2023. [Github](https://github.com/ysymyth/tree-of-thought-llm) (no code as of 22nd May)
* [Tree of Thoughts](https://github.com/kyegomez/tree-of-thoughts) Github, 21st May 2023


# Citations

Please cite this repository if you use the code.

    @misc{tree-of-thought-prompting,
        author = {Dave Hulbert},
        title = {Tree of Knowledge: ToK aka Tree of Knowledge dataset for Large Language Models LLM,
        year = {2023},
        publisher = {GitHub},
        journal = {GitHub repository},
        howpublished = {\url{https://github.com/dave1010/tree-of-thought-prompting}},
    }
