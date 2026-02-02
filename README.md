# Agentic Agent Factory using AutoGen Core

## Overview

This repository implements an **agentic agent-creation system** built using **AutoGen Core** and **AutoGen AgentChat**, where AI agents are capable of **creating, registering, and collaborating with other AI agents at runtime**.

The system is explicitly **agentic by design**, featuring agents that not only generate ideas but also dynamically spawn new agents with unique personalities, goals, and reasoning styles. These agents then collaborate by bouncing ideas between each other, simulating a creative, multi-agent ideation environment.

Rather than a static set of predefined agents, this project demonstrates how agent populations can **grow autonomously**, with agents refining each other’s outputs through structured message passing.

---

## Key Features

* **Dynamic agent creation at runtime**
* **Agent-to-agent collaboration and refinement**
* **AutoGen Core routing and messaging**
* **Personality-driven agent behaviour**
* **Parallel agent execution**
* **Persistent idea generation outputs**

---

## Tech Stack

* **Python**
* **AutoGen Core**
* **AutoGen AgentChat**
* **OpenAI Chat Models**
* **gRPC-based agent runtime**
* **Async execution with asyncio**

---

## How It Works

The system operates as an **agent factory and collaboration loop**, where one agent is responsible for generating new agents, and the generated agents then interact with each other to refine ideas.

At a high level, the workflow is:

1. A creator agent receives a template agent definition
2. The creator generates a new agent by modifying the template
3. The new agent is dynamically registered with the runtime
4. The newly created agent generates a business idea
5. The idea may be passed to another agent for refinement
6. Final ideas are written to disk as standalone artefacts

Multiple agents are created and executed **in parallel**, allowing for scalable idea generation.

---

## Code Architecture

### 1. Runtime Entry Point (`world.py`)

This file orchestrates the entire system.

Key responsibilities:

* Starts the gRPC agent runtime host
* Registers the creator agent
* Spawns multiple agent creation tasks concurrently
* Collects outputs from generated agents
* Writes final ideas to markdown files

This file controls scale and execution flow.

---

### 2. Base Agent Template (`agent.py`)

This file defines the **template agent** used as the blueprint for all newly created agents.

The agent:

* Has a strong personality and domain preferences
* Generates business ideas using agentic reasoning
* Optionally forwards ideas to other agents for refinement
* Responds with a final improved idea

This template is intentionally opinionated to encourage diversity in generated agents.

---

### 3. Agent Creator (`creator.py`)

The creator agent is responsible for **meta-agent behaviour**.

It:

* Receives the base agent template
* Uses an LLM to generate a new agent class
* Writes the new agent to a Python file
* Dynamically imports and registers the agent
* Immediately activates the agent and requests output

This demonstrates self-extending agent systems.

---

### 4. Messaging Layer (`messages.py`)

This module defines:

* A lightweight message schema
* Utilities for selecting other agents at random
* Agent discovery logic based on available files

This enables agents to collaborate without centralized coordination.

---

## Agentic Behaviour and Collaboration

A key design choice is that agents **may bounce ideas off each other**.

Each agent has a probability of:

* Sending its idea to another agent
* Requesting refinement or alternative perspectives
* Incorporating feedback into its final response

This leads to more diverse, creative, and less predictable outputs compared to single-agent generation.

---

## Outputs

Each generated agent produces a standalone idea that is written to disk as a markdown file:

```text
idea1.md
idea2.md
idea3.md
...
