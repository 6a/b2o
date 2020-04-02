---
layout: post
title: "Deal 'Em Down!"
date: 2020-01-27 23:01:16 +0000
categories: update
tags: update technology ue4 animation
---

One of the most important aspects of Blade II Online is the movement of cards around the arena. In order to do this in a modular, programmatic way, I opted to do all the animations entirely in code.

In order to do this, I had to develop a system that would allow each card to animate independantly from other cards, in complex ways. The system would need to be able to chain animations as well, while having variable inputs such as start/end state, duration, and travel path.

I created an actor component that stores references to cards on the board, and created one instance per slot (a specific area of the board where cards can be placed, such as the player/opponents deck, or their hands) on the board.

This actor component also provides multiple utility functions such as getting a card by index, or getting the target location for a card, again by index.

For example, the following function returns a position, and a rotation, for the Nth card in a particular slot.

```cpp
	/**
	 * Return the position + rotation that a card with the index "Index" should have.
	 * @param Index - The index for which the transform will be returned
	 * @returns FB2Transform for index "Index"
	 */
	const virtual FB2Transform GetTransformForIndex(UINT Index) const;
```

All of the actor components mentioned above are stored as memeber variables of the arena itself, to which the game mode instance stores a reference to at game start.

The game mode instance makes use of instances of classes such as a card factory, the aforementioned arena, and a dealer class. Using these, it can spawn, and then request the translation + animation of any cards on the board.

Once the game starts, the local game will, in theory, receive the initial state of the game (including the cards in each deck). However, as the networking stack, nor the game server, is implemented yet, I set up a test deck to see how the animations look.

```cpp
void ABladeIIGameGameModeBase::InitialiseBoard(B2BoardState BoardState)
{
	// Test state

	// Player Deck
	for (int i = 14; i >= 0; --i)
	{
		FB2Transform CardTransform = Arena->PlayerDeck->GetTransformForIndex(i);
		ACard* Card = CardFactory->Make(static_cast<ECard>(i % 11), CardTransform.Position, CardTransform.Rotation);
		Arena->PlayerDeck->Add(Card);
	}

	// Opponent Deck
	for (int i = 14; i >= 0; --i)
	{
		FB2Transform CardTransform = Arena->OpponentDeck->GetTransformForIndex(i);
		ACard* Card = CardFactory->Make(static_cast<ECard>(i % 11), CardTransform.Position, CardTransform.Rotation);
		Arena->OpponentDeck->Add(Card);
	}
}
```

As you can see, the cards have been created using the factory, and pointers have been added to the board's respective slots.

One added, it is safe to request for the cards to be dealt by the dealer instance. The implementation of this portion the deal function also highlights the way in which animations (transitions) are created, and added to the transition queue on a per-card basis.

```cpp
// Player cards
for (size_t i = 0; i < 10; i++)
{
    size_t DeckIndex = DECK_CAPACITY - 1 - i;
    size_t HandIndex = i;
    float Delay = 1.5f + i * 0.5f;

    // Transition 1
    FVector StartPosition = Arena->PlayerDeck->GetTransformForIndex(DeckIndex).Position;
    FRotator StartRotation = Arena->PlayerDeck->GetTransformForIndex(DeckIndex).Rotation;
    FVector EndPosition = Arena->PlayerDeck->GetTransformForIndex(DECK_CAPACITY - 1).Position + FVector(0, 0, 4.0);
    FRotator EndRotation = StartRotation;

    // Pop the topmost card from the players deck
    ACard* Card = Arena->PlayerDeck->RemoveByIndex(DeckIndex);

    // Add the transition to the transition queue
    B2Transition Transition = B2Transition(StartPosition, EndPosition, StartRotation, EndRotation, FVector(0, 0, 0), EEase::EaseInOut, 0.4f, Delay);
    Card->QueueTransition(Transition);

    // Transition 2
    StartPosition = EndPosition;
    StartRotation = EndRotation;
    EndPosition = Arena->PlayerHand->GetTransformForIndex(HandIndex).Position;
    EndRotation = Arena->PlayerHand->GetTransformForIndex(HandIndex).Rotation;

    // Add the transition to the transition queue
    Transition = B2Transition(StartPosition, EndPosition, StartRotation, EndRotation, FVector(0, -2.0, 12), EEase::EaseOut, 0.4f, 0.0f);
    Card->QueueTransition(Transition);

    // Add the card that we popped from the players deck, to the players hand
    Arena->PlayerHand->Add(Card);
}
```

And thus, the transition has been added to the card, and will be progressed as the game ticks. When the transition finishes, it will be removed from the queue, and the next transition will play (until there are no more transitions left).

To better illustrate what the B2Transition class does, please see its definition below:

```cpp
/**
 * Start a transition.
 * @param StartPosition - Starting position for this transition
 * @param EndPosition - Ending position for this transition
 * @param StartRotation - Starting rotation for this transition
 * @param EndRotation - Ending rotation for this transition
 * @param ArcOffset - How much the transition should arc in X, Y and Z.
 * @param Ease - The easing algorithm to use.
 * @param Duration - How long this transition should take
 * @param Delay - How long to wait before transitioning
 */
B2Transition(FVector StartPosition,
             FVector EndPosition, 
             FRotator StartRotation, 
             FRotator EndRotation, 
             FVector ArcOffset = FVector::ZeroVector, 
             EEase Ease = EEase::EaseInOut, 
             float Duration = 0.5f,  
             float Delay = 0.f
        );
```

The class itself is fairly simple; once configured, the tick function can be called with delta time to progress the animation, and public variables expose the current position and rotation for the transition (note - at this time it does not support scaling, but it may be added should the need arise).

And thus, after all the initial setup is complete, the resulting animation looks like this:

<video width="800" height="auto" controls="controls" autoplay loop>
  <source src="{{ site.url }}/media/vid/deal-em.mp4" type="video/mp4">
</video>
*Deal 'Em Down!*

This system will be used for all of the transitions that cards will make in Blade II Online.

Later on, I plan on adding extra features such as the ability to add callbacks at certain times, and a way to animate scalling (if required).

---

Note:
After a long, long break from working on this project, I have reached a point where I only have two deliverables left to hand in this year.

This means that I can actually spend time working on the development of Blade II Online, and of course, continue to update this blog!