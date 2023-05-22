# Using Tree-of-Thought Prompting to boost ChatGPT's reasoning

## Abstract

Tree-of-Thought (ToT) Prompting, a fresh technique borrowing ideas from the Tree-of-Thoughts framework, broadens and enhances the well-established Chain-of-Thought prompting concept, thereby enabling Large Language Models, like ChatGPT, to exhibit superior reasoning abilities. This Tree-of-Thought Prompting technique permits Large Language Models to rectify their errors autonomously while progressively accumulating knowledge.

In one example, a ToT prompt improves ChatGPT 3.5's reasoning ability to answer a question that could previously only be answered by ChatGPT 4.

## Complex questions for LLMs

Large Language Models (LLMs) often struggle to answer certain types of complex questions. Here's a representative example:

    Bob is in the living room.
    He walks to the kitchen, carrying a cup.
    He puts a ball in the cup and carries the cup to the bedroom.
    He turns the cup upside down, then walks to the garden.
    He puts the cup down in the garden, then walks to the garage.
    Where is the ball?

The correct answer is that the ball is in the bedroom. Although LLMs are non-deterministic, the responses I received during several attempts were as follows:

* ChatGPT 3.5: The ball is in the garden. ❌
* ChatGPT 4: The ball is in the bedroom. [followed by an explanation] ✅

## Chain-of-Thought prompting

A widely recognized concept known as "Chain-of-Thought prompting" (CoT for short) exists, which encourages Large Language Models (LLMs) to elucidate their thought process, thereby increasing the likelihood of a correct response. The roots of this concept can be traced back to a paper from January 2022 titled: [Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/abs/2201.11903).

At its core, the CoT technique can be implemented by adding something akin to the following to a prompt:

    Think carefully and logically, explaining your answer.

This approach enhances the performance of LLMs in answering many questions, although its effectiveness varies.

Here's an example of ChatGPT 3.5's response:

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

Recent weeks have seen the emergence of several papers and Python repositories introducing the concept of Tree-of-Thoughts (ToT):

* [Large Language Model Guided Tree-of-Thought](https://arxiv.org/abs/2305.08291), 15 May 2023. [Github](https://github.com/jieyilong/tree-of-thought-puzzle-solver).
* [Tree of Thoughts: Deliberate Problem Solving with Large Language Models](https://arxiv.org/abs/2305.10601), 17 May 2023. [Github](https://github.com/ysymyth/tree-of-thought-llm) (no code as of 22nd May)
* [Tree of Thoughts](https://github.com/kyegomez/tree-of-thoughts) Github, 21st May 2023

The underlying principle is to provide the LLM an opportunity to explore multiple branches of reasoning while self-evaluating as it progresses. According to the referenced papers, this approach seems to be highly effective for certain problems.

## Tree-of-Thought Prompting

TThe existing papers discussing the Tree-of-Thought framework appear to be premised on making multiple calls to an LLM, typically via Python. One of the main advantages of Chain-of-Thought reasoning is that it can be achieved with ChatGPT in a single sentence prompt.

This led me to ponder: could we improve upon the results of CoT by utilizing a ToT-style prompt?

Here's the prompt I used:

    Imagine three different experts are answering this question.
    All experts will write down 1 step of their thinking,
    then share it with the group.
    Then all experts will go on to the next step, etc.
    If any expert realises they're wrong at any point then they leave.
    The question is...

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

# Limitations

* This approach has not undergone extensive testing yet, but preliminary results indicate improved performance over traditional CoT prompts.
* The ToT framework comprises more components than those represented in my example prompt.


# Enhancement, Feedback, and Contributions

There's room to refine the prompt for improved performance, potentially leading to more concise responses.

Should you find success with any other prompts, we welcome you to submit a PR and add them to `tree-of-thought-prompts.txt`.

# Additional thoughts

The Tree-of-Thought framework bears resemblance to an **organizational decision-making process** involving multiple stakeholders.

High-performing teams often outperform individuals in decision-making. Therefore, it's plausible that adopting other organizational structures and characteristics could enhance the performance of LLMs.

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
