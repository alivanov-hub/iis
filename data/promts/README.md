# Microservice Boundary AI Agent - Prompts

This directory contains the prompt templates used to demonstrate the AI agent as an end-to-end solution for our Django monolith decomposition system. 

## Prompt 1: Zero-Shot Example
This prompt provides a single, new observation (X) and asks the agent to predict the microservice cluster (y).

**Input to AI:**
> You are an AI Architect Agent specializing in decomposing Django monoliths into microservices. 
> Analyze the following Django module's structural features (X) and assign it to the most appropriate microservice cluster (y).
>
> Input X:
> Module Name: `notifications`
> Features: [Imports: 15, FKs: 5, Celery Tasks: 12, DRF Serializers: 4]
> Dependencies:
> - Coupled to `users` (Code, ORM)
> - Coupled to `payments` (Framework - Celery tasks)
> 
> Process this data, explain your reasoning briefly, and provide the final output y (Microservice Cluster Name).


## Prompt 2: Few-Shot Example
This prompt extends the first one by providing known (X,y) mappings from our data set to guide the AI's predictions.

**Input to AI:**
> You are an AI Architect Agent specializing in decomposing Django monoliths into microservices. 
> Analyze the following Django module's structural features (X) and assign it to the most appropriate microservice cluster (y).
>
> Here are some examples of known mappings:
> Example 1:
> Input X: Module `payments`. Features: [Imports: 12, FKs: 29, Celery Tasks: 8, DRF Serializers: 15].
> Output y: "Financial/Billing Service"
> 
> Example 2:
> Input X: Module `homework_balancer`. Features: [Imports: 8, FKs: 14, Celery Tasks: 2, DRF Serializers: 10].
> Output y: "Educational Core Service"
> 
> Now, process the following new input:
> Input X:
> Module Name: `gamification_rewards`
> Features: [Imports: 9, FKs: 11, Celery Tasks: 5, DRF Serializers: 6]
> Dependencies:
> - Coupled to `homework_balancer` (Code)
> - Coupled to `users` (ORM)
>
> Process this data, explain your reasoning step-by-step, and provide the final output y.
