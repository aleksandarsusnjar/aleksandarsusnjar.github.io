---
layout: post
title: "REST API (2): Carving the state(s)?"
tags:
    - API
    - REST
    - RESTful
    - HATEOAS
    - Mythbusting 
    - "Software Development"
    - "Software Architecture"
categories:
    ask-yourself
permalink: /blog/rest-carving
excerpt: >
  In this post, I unravel the challenges of designing REST APIs, focusing on the often-overlooked complexities of carving state into meaningful resources. While REST may sound straightforward, the reality is full of trade-offs, missteps, and hidden intricacies that many designs fail to address. This isn't about blindly adhering to REST principles—it's about questioning assumptions, rejecting superficial solutions, and tackling the hard questions: Where do you draw the line between simplicity and overengineering? This post is your wake-up call to rethink what REST truly demands vs what it gives back.
---

Say you're designing an API and want it to follow the principles of REST. You'll need to make many decisions, which means asking yourself a lot of questions. I'll list many of those here in the hope that they help you avoid blind spots.

Before we dive in, let's make sure we're on the same page about what REST APIs actually are. You might be surprised, one way or another, by the variety of interpretations and implications. I covered this in [part 1](rest-what), so if you haven't seen it yet, I recommend taking a look.

# Resources?

![Map](/assets/rest-carving/chewing-gum-diagram.png){: style="float: right; width: 50%; margin-left: 1em;"}
What you're building will have a state - but how large is it? Since we're dealing with the transfer of state representations, would it be feasible to think of your entire system as a single resource and simply transfer its entire state back and forth between the client and server? Imagine sending a full database dump or its equivalent with every request.

I hope you're laughing in disbelief at the mere suggestion, which brings us to our first real question: how do we carve up that massive state into manageable pieces, what we'll call resources? It may feel like trying to separate blobs of stuck chewing gum, leaving behind stringy, tangled remnants. But we need to figure this out. To do so, let's consider the following:

- Do these chunks need to cover 100% of the monolithic state?
- Can different chunks overlap? How does that impact caching? Do we care?
- If elements in different chunks are related, where should the relationships live? One, some, or all of the related chunks? Or in a separate, dedicated chunks?
- How many chunks will be needed for any single use case?
- It's not just about data: what happens when consistent changes are needed across multiple chunks? (Think "transactions".) Can careful division prevent such a need?
- What are the implications of each of these decisions?

To help us align our assumptions, I'll introduce a concrete example: let's design an API for an interactive game of chess. The API will allow clients to make moves while maintaining the board state and enforcing the game's rules.

Fair warning: you may conclude that a REST API isn't the best fit for this scenario. That's intentional. We'll be exploring some of the challenges involved. Whether you find a better approach is up to you; I'll leave that as something to ponder. My only goal here is to help API designers avoid getting trapped by decisions that weren't thoroughly thought through.

# Rules of the Game

If you're already familiar with chess, feel free to skip this part - you know the rules. Otherwise, here's a quick rundown, highlighting some lesser-known quirks. No need to memorize anything; just get a sense of the types of rules that exist.

- The game is played on an 8×8 square board.

- Each player starts with 16 pieces of the same colour, with fixed initial positions:
  - 1 King
  - 1 Queen
  - 2 Rooks
  - 2 Knights
  - 2 Bishops
  - 8 Pawns
- Each piece moves differently. For example, pawns usually move forward one square at a time but not always. Kings are slow, moving just one square in any direction, but they can move two squares once in a special maneuver called "castling". Every piece has its own quirks.
- Players take turns making moves, and most moves affect just one piece... but not always:
  - Castling moves two pieces (the king and a rook) simultaneously in a way they normally couldn't, but only if neither has moved before.
  - If a piece moves onto a square occupied by an opponent's piece, that piece is "captured" and removed from the board.
  - When a pawn reaches the last rank (the opposite end of the board), it must be promoted to a queen, rook, bishop, or knight, whether or not that piece was previously captured. In theory, a player could end up with nine queens.
- "En passant" is a special rule that allows pawns to capture in a way that isn't immediately obvious from just looking at the board state. (I'll skip the details for now.)
- The game is won when the opponent's king is put in a position where it cannot escape capture - a "checkmate".
- The game can also end in a draw, which can happen in several ways:
  - Repeating the same moves three (or five) times.
  - Fifty consecutive moves without a capture or a pawn move.
  - Mutual agreement between players.
- A player may resign (admit defeat) or run out of time.

# What are we dealing with?

State-wise we have:

- players
- games, which there can be many, each choosing the variation of rules, time available and one, two or more players
- one board for each game
- up to 16 pieces for each of the two players
- positions of those pieces on the board
- game history (moves) that affect "En Passant", "Castling" and automatic game draw.
- remaining time for each player

Ignoring the setup, just to play the game we need to be able to:

- move one piece
- make a castling move
- pick a piece for the pawn promotion
- offer a draw
- be presented with a draw offer and accept it or decline it
- resign / accept defeat

# Carving Out Chess

![Map](/assets/rest-carving/broken-chessboard.png){: style="float: right; width: 50%; margin-left: 1em;"}
We now have a great mix of real-world challenges: complex states that aren't always easy to break down, non-trivial rules, transactions, hidden states, and elements that not everyone can be trusted with.

For now, let's assume we want our REST API to allow clients to transfer intended resource state representations to the server to play the game. **What types of resources would you design?** Let's try to stick to pure REST as defined in Roy Fielding's dissertation (see [part 1](rest-what)).

We'll explore a few illustrative possibilities, though not all of them. Where do your ideas fit in?

# Option 1: Game Monolith

This is the "funny" option I mentioned earlier, just scaled down to a game. Here, the state representation includes everything from the list above. To stay true to uniform interfaces and ensure we use the same representation for both the current and intended state (*Fielding 5.1.5* and *5.2.1.2*), the server should accept the entire state representation sent back for each move. That means including:

1. The positions of all pieces
2. The history of moves, or a digest covering captures, castlings, and pawn promotions (since these affect future possibilities, including whose turn it is)
3. Draw offers, acceptances, rejections, and resignations
4. The remaining time for each player
5. The players participating in the game
6. The board the game is played on
7. The chosen game variant

Now, think: what's the potential for abuse? Could a client modify the state to claim the opponent accepted defeat? Maybe they could simply (re)move pieces to their advantage, undo moves, hide parts of the history, or tamper with the remaining time.

Wait - time? Whose clock do we trust? What about time in transit?

This raises bigger questions about the division of power and responsibility. Does the server have any authority (or obligation) beyond merely validating whether the intended state is a legal progression from its current state? And if that's its role, how does it determine that, especially when multiple differences exist between the two?

# Option 2: Board Monolith

Let's separate the board, along with the pieces, into its own "board monolith" resource. Since clients can only manipulate one resource at a time, this eliminates the complexity of dealing with combined changes to both the board state and the broader game state. Even if you allow overlapping or composite resources (*Fielding 5.2.1.2*), each resource still needs to be managed independently, meaning you'd have to define the details of how they are transferred.

**What did we achieve? What new challenges arise?**

How many types of resources do clients now need to work with? I'd say one more than in option 1 since we've now split the board from the game. The game resource may still be needed for tracking variants, history, remaining time, and draw negotiations, while the board resource is required to see piece positions and execute moves.

How do we represent the relationship between the board and the game? Is it 1:1, or does the game maintain a history of boards?

- In a 1:1 model, each move simply modifies the existing board.
- In a 1:n model, each move creates a new board entry within the game.
- If boards are chained chronologically, does each board require a unique ID (and URL)?
- How does modifying an existing board versus creating a new one impact caching? How useful is caching in either case?

**Validation and Security**

The client can still submit an illegally modified board state to the server, meaning the server has to reverse-engineer the moves from its last known state to validate them. Does this process become any easier, now that the game state is implied rather than explicitly included? Even in chess, where moves are relatively easy to verify, this can get tricky. Now, imagine designing for a service where  validation isn't as straightforward.

**Statelessness & REST Constraints**

What does this mean for the statelessness of the API? Consider Fielding's note in *5.2.2*:

> "...each request contains all of the information necessary for a connector to understand the request, independent of any requests that may have preceded it."

Or *5.1.6*:

> "Within REST, intermediary components can actively transform the content of messages because the messages are self-descriptive and their semantics are visible to intermediaries."

Are we still adhering to these constraints, or have we introduced hidden state dependencies?

**Handling Concurrent Moves**

In multiplayer games, multiple players may attempt to make moves for the same turn and board state, even if it isn't their turn to do so. How do we recognize and resolve such conflicts?

We don't want them treated as separate moves.
In some cases, conflicting moves could even resemble an undo action, potentially triggering an automatic draw due to repetition.
Do we introduce optimistic locking? If we adopt a "game as a history of boards" model, we could use the previous board's ID as an expectation check. But then:

Who updates the previous board to point to the new one?
Is this starting to look like tighter coupling between the client and server?
How does this impact caching for game and board resources?
It's starting to feel like we've traded one set of complexities for another. But hey, that's the fun of API design!

# Option 3: Split out the pieces

Perhaps we should directly model the pieces that move? That seems to align with most moves, right? Now, we have:

- The game state (as a resource)

- Optionally, one or more board states representing positions over time

- 32 standard chess pieces (individual resources) on or off the board, plus additional pieces from pawn promotions

- Relationships: either the game or the boards must be linked to the pieces, both on and off the board, at any given position. Keep in mind that every relationship is bidirectional, regardless of how it's stored or represented.

Now, retrieving the initial game state would require 33 or 34 resources:

- 1 game resource

- 32 linked pieces

- potentially the board
 
Are you thinking about embedding linked resource states inside a composite resource that overlaps individual ones? If so, consider:

- Will you maintain uniformity, allowing manipulation of both the composite and its individual resources? Or is that just extra work? Would you sacrifice uniformity by making composites read-only? How should intermediaries behave if the same data can be communicated in multiple ways?

- Given a piece's state, can the client determine all possible moves without referring to data from prior requests? How does it know a move won't land on top of another piece from the same player?

- Given a new/intended piece state (position), what does the server need to look up to validate the move?

The request doesn't include all necessary data, conflicting with *Fielding 5.2.2*: *... each request contains all of the information necessary for a connector to understand the request, independent of any requests that may have preceded it.*

- What about moves affecting multiple pieces, such as captures, castling, or en passant? Should pawn promotion modify the piece's type, or should it exchange the pieces (removing the pawn and adding a new piece)? Would this require transactions? If so, how? Should we revisit composites? It's starting to feel like every design choice opens a new can of worms.

# Option 4: Moves are resources

OK, now we're moving away from resource = thing. An action is now a resource. Let's think about that:

- We can create new moves and read past ones.
- We can also create multi-piece / transactional moves.
- But what about modifying existing/past moves? No?

**Tracking Game State**


How do clients determine the positions of all pieces?
Do they need to replay all the moves, or will there be special resources to help?
If so, would these be read-only?
What are the caching implications?

**Consistency & REST Constraints**

Once again, what do we do about *Fielding 5.2.2*: *...each request contains all of the information necessary for a connector to understand the request, independent of any requests that may have preceded it*?

Does our approach violate this principle? If so, how do we justify or mitigate that?

**The Role of HTTP Methods**

Do you expect to stick with HTTP verbs (GET, POST, etc.) as the **only** action identifiers?

If so, does this approach feel like cheating or breaking that expectation?
If not, what's the alternative?
At this point, we're bending some classic REST ideas - maybe even breaking a few. But is that a problem, or just evolution?

# Option 5: I've got something better

You do? Maybe you do.

But how much effort did it take to figure it out?
Is it easy to explain to others? Would they understand?

Is it truly, fully REST?

Would you share it? Please do!

# Option 6: REST is something else

You might think I have a twisted view of REST or that some of the challenges I've listed don't really apply. I openly acknowledge that possibility!

If that's the case, I'd appreciate it if you read [part 1](rest-what) and help people like me understand it better. Just be ready for plenty of questions!

# Option 7:  REST isn't a fit

You don't like the other options and have decided to step outside REST principles. Maybe you'll have dedicated endpoints tailored to each specific use case instead of forcing square pegs into round holes. Perhaps part of your API will still be somewhat "REST-ish", like the state retrieval portion. You might even decide that caching is either impractical or irrelevant, or that statelessness isn't all that important after all.

Whatever approach you take, you can likely still document your API using the OpenAPI Specification. After all, it's about HTTP APIs, not just REST, so as long as you stick to HTTP, you should be fine. The desire to use standard tooling or other considerations may also lead you toward alternative API styles.

**Just to be clear: I'm not judging.** Why do you think I listed all these options?

# So what's the answer?

Did you come here looking for answers? Did you assume I had one to give? I created this site to encourage you to think for yourself and develop opinions you can understand and trust.

What about you? Do you know of a non-trivial, well-designed REST API in production that you can showcase? How does it handle the challenges discussed here? How much more is your server than just a data store and gatekeeper? Does it serve only one type of client, or many? If performance was a concern, how did you optimize it? Did you need to implement measures to detect and prevent malicious requests, such as denial-of-service attacks? How did you approach that?

Or maybe you fall into a different group, one that has never built, used, or even seen a proper REST API, despite working with systems that carry those letters in their name. Does that sound familiar?

See you in the next post!


# Next...

Continue to [part 3](rest-tug-of-war) to think about the tug of war fought by the opposing requirements.