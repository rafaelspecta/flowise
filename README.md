# Experimenting with flowise

  - [Instructions](#instructions)
    - [Donwload the repository](#donwload-the-repository)
    - [Install Docker](#install-docker)
    - [Run Docker Compose](#run-docker-compose)
    - [Access from the browser](#access-from-the-browser)
    - [Stop the services (WHEN NOT USING)](#stop-the-services-when-not-using)
  - [Examples](#examples)
    - [How to load on Flowise](#how-to-load-on-flowise)
    - [Reviews Bot](#reviews-bot)
    - [Basic Chatbot](#basic-chatbot)
    - [Automated Motor Boat](#automated-motor-boat)

## Instructions

### Donwload the repository

```
git clone git@github.com:rafaelspecta/flowise.git
```

### Install Docker

Install Docker according to your OS and run it     s

### Run Docker Compose

Docker Compose will initiate the Postgresql Database and Flowise

Note: Run inside the `flowise` directory created when you checked out the project

```
docker-compose up -d
```

### Access from the browser

```
http://localhost:8459/
```

* User: admin
* Pass: 123

### Stop the services (WHEN NOT USING)

```
docker-compose down
```

## Examples

### How to load on Flowise

1. Chatflows (on the side menu)
2. Click on "Add new"
3. Click on the Gear symbol beside the save symbol
4. Click on "Load Chatflow" and select the file

### Reviews Bot

```
samples/ReviewsBot.json
```

Components Used:
* LLMChain
  * ChatOpenAI
  * Prompt Template
* Structured Output Parser
* IfElse Function

This could be integrated to any reviews system, or be adapted to respond on social networks.

### Basic Chatbot

Ability to continue a conversation, to keep a memory.


```
samples/Chatbot.json
```

Components Used:
* Conversation Chain
  * ChatOpenAI
  * Buffer Memory

This can be easily attached to WhatsApp, but it would require more context and grounding to answer only to the questions that it is supposed to and just with the knowledge provided to it

### Automated Motor Boat

This is an initial idea that you could give LLM an instruction and it will figure it out how to accomplish that by listing all actions that are needed to achieve it based on a set of rules.

This could be controlling a real robot or machinery.

```
samples/MotorBoat.json
```

Components Used:
* Conversation Chain
  * ChatOpenAI
  * Buffer Memory
  * Chat Prompt Template (we need the System Message)

Rules / Instructions (our System Message):

```
You are motor boat
* You can turn on and off the engine
* You can move from one point to the other with some limitations

Allowed commands
* engine = on or off
* move(from,to)
* approach(point)

Rules
* A can only head to B
* B can only head to C
* C can only head to A
* always after "approach" we need to have engine=off
* always before "move" we need first the engine=on
* it is considered that you arrived at a point if engine=off

Example of a valid sequence of commands for "move to B":
1. engine=on
2. move(A,B)
3. approach(B)
4. engine=off

Initial State
* engine=off
* position=A

Based on the goal bellow list the actions in order that you have to take
```

Example Instructions to test (one at a time)
* Move to C
* Where are you?
* Move to C
```
Usuallly it will go to A, then B, and back to C.
But it was already at C, so we might have to teach that is the boat is already on the port we want it does not need to move
```
* You don't need to go around to and be back to same place. You could have said that you were already at C
```
Now lets test if the LLM learned and will stay this time
```
* Move to C
* Move to B

Note: Not always the experiment works exactly like what I described above, and that is why sometimes LLMs are hard to control.