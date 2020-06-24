# In the beginning, there was nought.

So, you've been playing Minecraft for 12 years now, and you're getting bored of exploring caves. You heard about this new fad, called 'modding', whatever that means, and you want in.

Welcome! This is going to be the first step of your extensive journey through the wilds of Minecraft and the mad mad world of modding. We'll begin with Java Edition, modding with Forge. If the brave adventurers following this path manage it far enough, we'll venture into more advanced topics.

It's dangerous to go alone, so i've prepared this quick-fire introduction to Java, and the Forge way of doing things:

## The Ethos

Forge is a monolithic modloading library for Minecraft - that meaning, it allows a lot of adventurers to add content just how they like it, without interfering with each other. They can still talk to each other despite being held back from the world (game) itself. 

The reason we're allowed to do this is that Forge acts as an intermediary. We tell Forge that we want to know when the game is doing certain things (In Forge-speak, when certain events are fired), and it'll remember to let us know. We can do what we want if the time is right, and the coordination skills of Forge means that any amount of mods can do the same, and the cumulative results of every mods' changes are applied to the game.

Say, you want to add a grenade - that when thrown and lands on an object, explodes and damages the blocks and entities around it. To do that, you'd subscribe for the registry events - ask Forge to tell us when the game and other mods are adding things to the game so that we can jump on the bandwagon - to add the item and the entity (for the thrown grenade!).

You'd then subscribe for the "player right clicks an item" event, create a grenade entity with speed in the direction the player is looking (so that your grenade is actually thrown) and with gravity applied (like an arrow!). Then, when the arrow lands, summon an Explosion entity which handles the rest.

As a matter of fact, let's work toward creating such an item, as your introduction to the mad, mad world you've found yourself in.

# Before we start

This tome assumes that you have a minimum viable working knowledge of the Java language.
If you do not, here are resources to help you:

Official documentation:
https://docs.oracle.com/javase/tutorial/
Absolute basics with an interactive editor:
https://www.codecademy.com/learn/learn-java
Ongoing online course with assignments:
https://java-programming.mooc.fi/

