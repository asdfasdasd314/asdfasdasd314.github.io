```markdown
# James Hollingsworth — Projects

Computer Science student at the University of Michigan interested in AI systems, software engineering, and building useful products.

## Projects

- [AI Coding Environment](YOUR_REPOSITORY_LINK)
  - Long-term memory, codebase understanding, and multi-agent orchestration for AI coding agents.

- [Prediction Market Trading Bot](YOUR_REPOSITORY_LINK)
  - Automated system for identifying and trading price discrepancies across prediction markets.

- [Medley](YOUR_REPOSITORY_LINK)
  - Personalized music recommendation system that models both musical characteristics and a user's personal associations with songs.

- [Music Chord Classifier](YOUR_REPOSITORY_LINK)
  - PyTorch transformer for classifying musical chords from audio in real time.

---

# AI Coding Environment

## Problem Statement

**How can I reduce token usage and bugs when using AI agents to work on large codebases?**

Modern coding agents are effective when given enough context, but providing that context becomes increasingly expensive as a codebase grows. At the same time, agents can introduce bugs when they lack an understanding of how the feature they are modifying relates to the rest of the system.

The goal of this project was to build an environment around coding agents that gives them persistent, structured knowledge of a codebase while minimizing how much raw code needs to be repeatedly placed into their context.

## Realizations

### 1. The codebase itself should not be the agent's only memory

An agent frequently needs information that is distributed across many files. Repeatedly rediscovering that information wastes tokens and can cause different agents to develop inconsistent understandings of the same system.

I began building a long-term memory layer that maintains structured information about the codebase so that relevant context can be retrieved instead of rediscovered for every task.

### 2. Code is naturally a graph

Files alone are not a particularly useful representation of software architecture. Functions call other functions, classes depend on other classes, data flows through multiple components, and individual features can span many files.

This led to **Graphify**, which represents relationships between functions, classes, dependencies, and higher-level features as a graph. Instead of asking an agent to inspect an entire repository, the system can identify the portion of the graph relevant to a task and provide that context.

### 3. Independent agents need isolated environments

Running multiple coding agents simultaneously creates another problem: they can modify overlapping parts of the repository.

Git worktrees provide each agent with an isolated working copy and branch while still sharing the same underlying repository. This makes it possible to execute tasks in parallel and merge their changes afterward.

### 4. Merge conflicts can become an agent task themselves

Parallel agents inevitably produce conflicting changes.

Rather than requiring every conflict to be manually resolved, the orchestration layer can provide the conflicting changes and their surrounding context to another agent responsible for reconciling them.

## Issues

### Determining what context is actually relevant

Providing too little context causes mistakes, but providing too much defeats the purpose of the system. One of the central challenges is determining which symbols, dependencies, features, and pieces of historical information should be included for a particular task.

### Keeping long-term memory synchronized with the code

Stored architectural knowledge becomes harmful if it no longer represents the actual codebase. The system therefore needs to detect architectural changes and update its representation without rebuilding its understanding of the entire repository after every edit.

### Defining features automatically

Functions and classes can be extracted mechanically, but a "feature" is a semantic concept. A single feature may involve several files and functions, while one file may implement multiple unrelated features.

Determining these boundaries automatically remains one of the more difficult parts of generating useful high-level context.

## Final Result

The project is an AI coding environment that combines persistent codebase memory, architectural graphs, Git worktrees, and agent orchestration to provide coding agents with targeted context while allowing multiple agents to work in parallel. The resulting system is designed to reduce redundant token usage and prevent bugs caused by agents operating without sufficient knowledge of the surrounding codebase.

---

# Prediction Market Trading Bot

## Problem Statement

**How can I profit from prices that differ across prediction market exchanges?**

Prediction markets such as Kalshi and Polymarket can offer contracts representing effectively the same underlying event while assigning different prices to those outcomes. When the combined cost of complementary positions across the two exchanges falls below their guaranteed payout, the difference creates a potential arbitrage opportunity.

The goal was to automatically identify these discrepancies and execute both sides of a trade before the opportunity disappeared.

## Realizations

### 1. Equivalent markets do not necessarily have equivalent prices

Separate exchanges have different participants, liquidity, order books, and market structures. As a result, contracts representing the same event can temporarily trade at different implied probabilities.

This means the system can compare equivalent contracts across exchanges rather than attempting to predict the actual outcome of an event.

### 2. Finding an opportunity is easier than executing it

A profitable price discrepancy can disappear quickly. By the time one order executes, the price on the second exchange may have changed.

This shifted the problem from simply detecting arbitrage to building a sufficiently fast and reliable execution system.

### 3. Order books matter more than displayed prices

A quoted price does not guarantee that enough contracts are available at that price.

The bot therefore needs to consider available liquidity and determine how many contracts can actually be purchased before calculating the expected profit of an opportunity.

### 4. Asynchronous execution reduces latency

Waiting for network requests and exchange APIs sequentially wastes valuable time during a short-lived opportunity.

Using asynchronous operations allows the system to monitor markets and submit or manage orders concurrently, reducing the delay between detecting an opportunity and attempting to capture it.

## Issues

### Matching equivalent markets

Two exchanges can describe the same underlying event differently. The system needs to determine whether two contracts actually have equivalent settlement conditions before treating their prices as an arbitrage opportunity.

### Execution risk

The largest practical risk occurs when one side of an arbitrage executes and the other does not. What initially appeared to be a risk-free trade can then become an exposed directional position.

### Liquidity

Some apparent opportunities exist only for a small number of contracts. Expected profitability therefore needs to account for both the price discrepancy and the quantity actually available in each order book.

### Latency

Opportunities can disappear between detection and execution. Network latency, API response times, order submission, and order-book changes all affect whether a theoretically profitable trade can actually be captured.

## Final Result

The final system monitors equivalent prediction markets across Kalshi and Polymarket, evaluates cross-exchange price discrepancies, and asynchronously manages limit orders to capture viable arbitrage opportunities. The system was deployed with real capital and generated approximately **$200 in profit**.

---

# Other Projects

## Medley

[View Repository](YOUR_REPOSITORY_LINK)

Medley is a personalized music recommendation system integrated with the Spotify API. It models both musical properties and personal associations with songs, allowing AI agents to reason about connections such as memories, people, places, moods, and experiences when recommending music.

## Music Chord Classifier

[View Repository](YOUR_REPOSITORY_LINK)

A PyTorch transformer neural network designed to classify musical chord symbols from audio in real time. The system uses Fourier transforms and audio-processing techniques to convert raw audio into a representation suitable for classification.
```
