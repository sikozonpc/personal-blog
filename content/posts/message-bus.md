---
title: "Handling game events with an Event Message Bus"
date: 2020-06-28
draft: false
description: "Improve your game code by decoupling object references with an event based system"
tags: [csharp, game-dev, computer-science]
---

### The problem

If you use/used [Unity 3D](https://unity.com/) you might have seen this problem: you need to interact with other objects in your project
and so you add a public reference to them or even find it with `FindObjectOfType` and just call the method you need or get the data you want.
So far so good, but your projects starts growing and something changes and suddenly everything breaks or worst "Silently breaks".
Not to mention it get's hard to read.

It's alright to have been part of this problem, I was. My earliest projects were like this, and even official Unity tutorials teach like this
for simplicity.

However, in this comunity people consume a lot of tutorials, blog posts and even courses that teach and force component coupling.
And so with this post my aim is to give my 2 cents on how to avoid this common mistake.

### Background

Unity, is a component driven game engine, it utilizes the [Component Pattern](https://gameprogrammingpatterns.com/component.html) for it's development, where each component is a part of 
a bigger system and has it's own specific job (ideally) and can be attached to any game object to add functionality ore behaviour.

A good component is a component that nicely fits into the system and is also unaware of the system.

In my opinion, this is what makes Unity so flexible and accessible.


### Solution

My proposed solution is to build a Obeserver/Subscriber messaging system where the components broadcast **events** and the system notifies the subscriebed components about that event and so 
they act accordingly. (It's basically a facade for an implementation of the Obeserver Pattern)

In theory this solves the problem of object coupling, this compoonents will only need to access this singleton to send messages `MessageBus.SendMessage(Message msg)` or to 
subscribe to messages `MessageBus.AddSubscriber(MessageSub msgSub)`.


#### Messages

These contain the data that is communicated and an ID to identifier the message type, I would recommend having a enum for the message ID since 
strings are not unique and can easyly be misstyped.

 
```c#
public enum MessageType
{
    NONE,
    ACTIONBAR_SELECTED_INDEX_CHANGED,
    INVENTORY_CHANGED,
    ACTIONBAR_INDEX_CHANGED,
}

public struct Message<T>
{
    public MessageType Type;
    public T Data;
}
```

Another clever approach is to use Unity's [Scriptable Object](https://docs.unity3d.com/Manual/class-ScriptableObject.html) as the messages.

### The Message Bus singleton

This is going to be the facade responsible for exposing the methods for the components to consume. I won't go too deep into how to implement since I would highly recommend 
[this approach by franciscotufro](https://github.com/franciscotufro/message-bus-pattern).

But as an overview the Message bus hold a collection of the messages to a list of subscribers:

```c#
    Dictionary<MessageType, List<MessageSubscriber>> subscriberLists =
        new Dictionary<MessageType, List<MessageSubscriber>>();
```

and the rest are the public methods to `AddSubscriber` and `SendMessage`

### Turning game objects into listeners

A very cool thing you even do with Unity is create a component to be a `MessageSubscriberController` and with it you can attach to game object and define 
the events on the Unity editor itself, it's the way to make this game object a **message listener**


### Conclusion

With this small essay I just wanted to share this pretty cool solution that might save hours of debugging and bring joy to some programmers.

Hope you enjoyed and learnt something. Don't forget to checkout the references & more reading bellow!

### References & more reading

- [Component Pattern from gameprogrammingpatterns](https://gameprogrammingpatterns.com/component.html)
- [How to architect your code as your project scales form Unity](https://unity3d.com/pt/how-to/architect-code-as-your-project-scales)
- [Message bus pattern aproach by franciscotufro](https://github.com/franciscotufro/message-bus-pattern)
