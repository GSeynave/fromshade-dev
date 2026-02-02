---
title: '1 - Overloading the websocket'
type: 'Progress'
description: 'Web dev doing game dev.'
pubDate: 'Feb 02 2025'
tags: ['Game Dev', 'System Design']
enable: false
---


  
  <!-- - Client handles stack of event one by one 
    - Event Queue Stack 
    - Progress in event (Duration / frame)
    - Mistake of add each event to its entity -> leading to loosing the order of each actions !
- Some architecture concept to easily integrate new GameEvent / action -->



Project overview: [Autobattle project](../03-autobattle-project)


## The server execute and notify, the client render.


The game is passive for player but a lot happens in the background.
The server needs to compute each possible actions, such as the player move position, the player attack a target, a target is moving, ...

The client is responsible to turn those background action into nice visuals for the player.


### How does the server communicate with the client ?


My first mistake was to assume the server will notify the client through the websocket as soon as an action is made.

![Client_Overload](../../assets/Game-01/client_overload.png)

At first it was very straightforward, the server moved my character, then notify the client through the websocket : The character move from x to y.
Then the player attack, notification : The Character attack X with Y damage.
But those actions are very "short" time-wise. This was leading to a lot of message within a single second and it was only with 1 Character and 1 Target !

Later, you could already see the problem, to have a bunch of enemies, a full team of characters on the fields, even more possible actions.. The websocket would not handle it.

I had to solve my first conception problem : How to keep the game running in both the server and the client without overloading the websocket.

**Create a queue of actions**. And here was the solution for my first problem, batch actions.


![Batch_Action](../../assets/Game-01/batch_action.png)

The server need to queue actions and send a message to the client with a fix rate with the pile of actions. This involves rethinking the way we handle actions.

Previously it was very simple, the server apply an action and send the action to the client with the duration. The client rendered it in the given duration.
Now with a queue of actions it's different.
The client need to take them one by one and to calculate its progress to display a smooth action.
More on that later.

![Tick_System](../../assets/Game-01/tick_system.png)

Take a look here, the game never stop but server now time the message to client by 'TICK'.
A tick is just the fact that we will trigger a message after a given time.

Let's walk through this.

Here the tick is 1500ms.

Tick 1 :
The server begin by resolving the next action, add it to the queue, and remove the time needed for the action to the current tick. Here the Moev action of Tick 1 is 1500ms. Meaning that after this action the tick is already filled. So we send the message to the client with : "During this tick you have to apply : Move".

Tick 2 :
The server resolve the action : Attack is 500ms. So 1000 ms left for this tick.
The server continue, Attack is 500 ms also. There is 500 ms left in this tick.
Move is 1500 ms: And now we don't have any time left in this tick. The server send the queue.

Tick 3 :
Here it's a bit different.
See how I put 2 attacks in grey? If the server couldn't pass information in between tick, this is what it would have done. Indeed, a tick is 1500 ms, then Tick 3 need to pile up 1500 ms of actions.
The server receives those actions and render them in the given time.
But wait! Tick 2 ended with a long movement action, then the client would still be rendering the movement that the server would think that it is rendering the attack. Then come the tick 4 with even more actions and you see where it goes, the client begins to be late.

The solution is to carry over the time used into the next tick.
Looking at the example, the server will assume only 500ms is available when processing the tick3, resulting in adding a single attack action. They grey one are not even considered anymore.



Coming next : Scheduling action, vector manipulation, queue event in the client side, browser throttling

**Previous post: [From Shade](../00-story-From_Shade)**
