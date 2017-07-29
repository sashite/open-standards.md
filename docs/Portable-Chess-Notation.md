# Portable Chess Notation <small>Specification and Implementation Guide</small>

A general purpose JSON-based format for recording chess variants' games.

<dl class="dl-horizontal">
  <dt>Created</dt>
  <dd><time datetime="2012-08-05T01:23:45Z">August 5, 2012</time></dd>

  <dt>Updated</dt>
  <dd><time datetime="2017-07-28T23:42:34Z">July 28, 2017</time></dd>

  <dt>Status</dt>
  <dd>beta</dd>

  <dt>Author</dt>
  <dd><a rel="external author" href="https://github.com/cyril">Cyril Kato</a></dd>
</dl>

<div class="alert alert-warning">
  <button type="button" class="close" data-dismiss="alert">&times;</button>
  <strong>This is a work in progress!</strong>
  This is a beta document and may be updated at any time.
</div>

***

<div class="sub-title">Abstract</div>

This document proposes a format for representing most **chess** games (both the _moves_ and related data).

<div class="sub-title">Status of this document</div>

This document is a beta of the PCN specification.

<div class="sub-title">Copyright Notice</div>

The content of this page is licensed under the [Creative Commons Attribution 3.0 License](//creativecommons.org/licenses/by/3.0/), and code samples are licensed under the [Apache 2.0 License](//www.apache.org/licenses/LICENSE-2.0).

***

## Introduction

<abbr title="Portable Chess Notation">PCN</abbr> (<q>Portable Chess Notation</q>) is a lightweight format based on <abbr title="JavaScript Object Notation">JSON</abbr> and <abbr title="Portable Board Diff Notation">PBDN</abbr> that gives a consistent and easy way to represent most chess games between two-players.

Able to describe both multidimensional starting positions and related moves, easy for humans to read and write, and easy for machines to import and export, it is completely laws of chess independent and compatible with the main variants, including [<ruby lang="ko">장기<rt lang="en">Janggi</rt></ruby>](//en.wikipedia.org/wiki/Janggi), [<ruby lang="th">หมากรุก<rt lang="en">Makruk</rt></ruby>](//en.wikipedia.org/wiki/Makruk), [<ruby lang="ja">将棋<rt lang="en">Shogi</rt></ruby>](//en.wikipedia.org/wiki/Shogi), [Western](//en.wikipedia.org/wiki/Chess), [<ruby lang="zh">象棋<rt lang="en">Xiangqi</rt></ruby>](//en.wikipedia.org/wiki/Xiangqi).  These properties make PCN an ideal data-interchange format for recording chess games.

### Notational conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](//tools.ietf.org/html/rfc2119).

#### Naming the actors

The key word "actor" and its notation are to be interpreted as described in [General Actor Notation](General-Actor-Notation).

## Specification goals

A specification for a portable chess notation MUST observe the following criteria:

1. The details of the system MUST be publicly available and free of unnecessary complexity.
2. The details of the system MUST be non-proprietary so that users and software developers are unrestricted by concerns about infringing on intellectual property rights.
3. The system MUST work for a variety of programs.  The format SHOULD be such that it can be used by chess database programs, chess publishing programs, chess server programs, and chessplaying programs without being unnecessarily specific to any particular application class.
4. The system MUST handle games which are playable by two sides.
5. The system MUST be easily expandable and scalable.  Examples: new chessboards, new opening positions, new actors, new moves, new rules, etc.
  * The system MUST be cross-variants.
    It SHOULD be able to represent games where sides MAY play
    different chess variants such as Janggi, Makruk, Shogi, Western, Xiangqi.
  * The system MUST handle chessboards of any dimension.
    Thus, it SHOULD be able to represent board dimensions such as: 2D, 3D, etc.
  * The system MUST handle chessboards of any size.
    Thus, it SHOULD be able to represent board sizes such as: 8x8, 9x9, 9x9x9, etc.
  * The system MUST be able to fully describe moves according to
    a concerned Laws of Chess and the given chessboard structure.
6. Finally, the system SHOULD handle the same kinds and amounts of data that are already handled by existing chess software and by print media.

## How to serve PCN

The use of the file suffix "`.pcn`" is RECOMMENDED for files containing PCN data.

When serving PCN over HTTP, the media type "`application/vnd.pcn+json`" is RECOMMENDED.

## <span id="resource">Notation for Games</span>

<div class="sub-title">Resource representation</div>

The JSON structure below shows the format of the resource:

    {
      "topside": {a hash},
      "bottomside": {a hash},
      "state": {a string},
      "started_at": {a datetime},
      "indexes": {an array},
      "startpos": {an array},
      "moves": {an array}
    }

<div class="sub-title">Properties</div>

The following table defines the properties that appear in this resource:

| Property name   | Value      | Description |
| --------------- | ---------- | ----------- |
| "`topside`"     | a hash     | The name of topside style. |
| "`bottomside`"  | a hash     | The rating of bottomside. |
| "`state`"       | a string   | Indicates the state of the game. |
| "`started_at`"  | a datetime | The date and time that the game was started.  The value is specified in [ISO 8601](//www.w3.org/TR/NOTE-datetime) format. |
| "`indexes`"     | an array   | The shape of the board. |
| "`startpos`"    | an array   | List of squares that identify actors on the board's starting position. |
| "`moves`"       | an array   | List of moves associated with the game. |

### Top-side & Bottom-side

The reserved "`topside`" & "`bottomside`" properties are RECOMMENDED.

<div class="sub-title">Properties</div>

The following table defines the properties that appear in this resource:

| Property name       | Value               | Description |
| ------------------- | ------------------- | ----------- |
| "`name`"            | a string            | The name of the player. |
| "`rating`"          | an unsigned integer | The rating of the player. |
| "`remaining_time`"  | an unsigned integer | The remaining time of the player (in seconds). |

### State

The reserved "`state`" property is REQUIRED.

The possible values are (when the game is finished):

* "`topside_won`"
* "`bottomside_won`"
* "`drawn_game`"

and (when the game is not finished):

* "`in_progress`"
* "`draw_request_from_topside`"
* "`draw_request_from_bottomside`"

### Starting datetime

The reserved "`started_at`" property is RECOMMENDED.

The "`started_at`" property specifies the starting datetime for the game.

The value is specified in [ISO 8601](//www.w3.org/TR/NOTE-datetime) format.

### Starting position

The reserved "`startpos`" property is REQUIRED.

#### Board

Chess positions on the chessboard can be represented by an array.

Even though it is unusual, it is possible to represent a chessboard from one dimension.  Each dimension can be described by an embedded array (where the first dimension SHOULD be the root array).

##### Empty position examples

###### 6 size chessboard

    [ null, null, null, null, null, null ]

###### 4x8 size chessboard

    [
      [ null, null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null, null ]
    ]

Example of suitable game: [<ruby lang="zh">盲棋<rt lang="en">Banqi</rt></ruby>](//en.wikipedia.org/wiki/Banqi).

###### 7x7 size chessboard

    [
      [ null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null ]
    ]

Example of suitable game: [<ruby lang="ja">禽将棋<rt lang="en">Tori Shogi</rt></ruby>](//en.wikipedia.org/wiki/Tori_shogi).

###### 5x5x5 size chessboard

    [
      [
        [ null, null, null, null, null ],
        [ null, null, null, null, null ],
        [ null, null, null, null, null ],
        [ null, null, null, null, null ],
        [ null, null, null, null, null ]
      ],
      [
        [ null, null, null, null, null ],
        [ null, null, null, null, null ],
        [ null, null, null, null, null ],
        [ null, null, null, null, null ],
        [ null, null, null, null, null ]
      ],
      [
        [ null, null, null, null, null ],
        [ null, null, null, null, null ],
        [ null, null, null, null, null ],
        [ null, null, null, null, null ],
        [ null, null, null, null, null ]
      ],
      [
        [ null, null, null, null, null ],
        [ null, null, null, null, null ],
        [ null, null, null, null, null ],
        [ null, null, null, null, null ],
        [ null, null, null, null, null ]
      ],
      [
        [ null, null, null, null, null ],
        [ null, null, null, null, null ],
        [ null, null, null, null, null ],
        [ null, null, null, null, null ],
        [ null, null, null, null, null ]
      ]
    ]

Example of suitable game: [Raumschach](//en.wikipedia.org/wiki/Three-dimensional_chess#Raumschach).

##### Starting position examples

###### Janggi chessboard

    [
      [ "j:r", "j:m", "j:e", "j:s", null, "j:s", "j:e", "j:m", "j:r" ],
      [ null, null, null, null, "j:^g", null, null, null, null ],
      [ null, "j:p", null, null, null, null, null, "j:p", null ],
      [ "j:j", null, "j:j", null, "j:j", null, "j:j", null, "j:j" ],
      [ null, null, null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null, null, null ],
      [ "J:J", null, "J:J", null, "J:J", null, "J:J", null, "J:J" ],
      [ null, "J:P", null, null, null, null, null, "J:P", null ],
      [ null, null, null, null, "J:^G", null, null, null, null ],
      [ "J:R", "J:M", "J:E", "J:S", null, "J:S", "J:E", "J:M", "J:R" ]
    ]

###### Makruk chessboard

    [
      [ "m:r", "m:n", "m:b", "m:q", "m:^k", "m:b", "m:n", "m:r" ],
      [ null, null, null, null, null, null, null, null ],
      [ "m:p", "m:p", "m:p", "m:p", "m:p", "m:p", "m:p", "m:p" ],
      [ null, null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null, null ],
      [ "M:P", "M:P", "M:P", "M:P", "M:P", "M:P", "M:P", "M:P" ],
      [ null, null, null, null, null, null, null, null ],
      [ "M:R", "M:N", "M:B", "M:^K", "M:Q", "M:B", "M:N", "M:R" ]
    ]

###### Shogi chessboard

    [
      [ "s:l", "s:n", "s:s", "s:g", "s:^k", "s:g", "s:s", "s:n", "s:l" ],
      [ null, "s:r", null, null, null, null, null, "s:b", null ],
      [ "s:p", "s:p", "s:p", "s:p", "s:p", "s:p", "s:p", "s:p", "s:p" ],
      [ null, null, null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null, null, null ],
      [ "S:P", "S:P", "S:P", "S:P", "S:P", "S:P", "S:P", "S:P", "S:P" ],
      [ null, "S:B", null, null, null, null, null, "S:R", null ],
      [ "S:L", "S:N", "S:S", "S:G", "S:^K", "S:G", "S:S", "S:N", "S:L" ]
    ]

###### Western chessboard

    [
      [ "w:r", "w:n", "w:b", "w:q", "w:^k", "w:b", "w:n", "w:r" ],
      [ "w:p", "w:p", "w:p", "w:p", "w:p", "w:p", "w:p", "w:p" ],
      [ null, null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null, null ],
      [ "W:P", "W:P", "W:P", "W:P", "W:P", "W:P", "W:P", "W:P" ],
      [ "W:R", "W:N", "W:B", "W:Q", "W:^K", "W:B", "W:N", "W:R" ]
    ]

###### Xiangqi chessboard

    [
      [ "x:r", "x:h", "x:e", "x:a", "x:^g", "x:a", "x:e", "x:h", "x:r" ],
      [ null, null, null, null, null, null, null, null, null ],
      [ null, "x:c", null, null, null, null, null, "x:c", null ],
      [ "x:s", null, "x:s", null, "x:s", null, "x:s", null, "x:s" ],
      [ null, null, null, null, null, null, null, null, null ],
      [ null, null, null, null, null, null, null, null, null ],
      [ "X:S", null, "X:S", null, "X:S", null, "X:S", null, "X:S" ],
      [ null, "X:C", null, null, null, null, null, "X:C", null ],
      [ null, null, null, null, null, null, null, null, null ],
      [ "X:R", "X:H", "X:E", "X:A", "X:^G", "X:A", "X:E", "X:H", "X:R" ]
    ]

### Moves <small>on the chessboard</small>

The reserved "`moves`" property is REQUIRED.

The moves themselves are given as an ordered set of _actions_ in [portable board diff notation](Portable-Board-Diff-Notation).  A move MUST contain at least one action.

For example, given the following move composed of 1 action:

    [[ 42, 43, "S:+R" ]]

It can be translated into:

> Move from the coordinate 42 to the coordinate 43 the <q>Shogi promoted Rook</q>.

<div class="alert alert-info">
  Depending of the context, given by the board and the state of the game, this move MAY be a capture, it MAY be concluded by a promotion, and it MAY be a drop.
</div>

<div class="sub-title">Several scenarios</div>

#### Drop <small>a Shogi Rook to square 2</small>

Captured actors can be truly _captured_, such as in Shogi.  Thus, they are retained <q>in hand</q>, and can be brought back into play under the capturing player's control.  On any turn, instead of moving an actor on the board, a player MAY take an actor that had been previously captured and place it, unpromoted side up, on any empty square, facing the opposing side.  The actor is then part of the forces controlled by that player.  This is termed _dropping_ the actor, or just a _drop_.

Given the following position:

    [
      [ null, null, null, null, null, null ],
      [ null, null, null, null, null, null ],
      [ null, null, null, null, null, null ],
      [ null, null, null, null, null, null ]
    ]

When this move, consisting of one action, is played:

    [[ 2, 2, "S:R" ]]

Then the position becomes:

    [
      [ null, null, "S:R", null, null, null ],
      [ null, null, null, null, null, null ],
      [ null, null, null, null, null, null ],
      [ null, null, null, null, null, null ]
    ]

<div class="alert alert-info">
  By default, the source square of the dropped actors SHOULD be equal to the destination square.
</div>

#### Capture <small>an actor</small>

Let's detach an opponent's actor from the board by taking it with a friendly actor:

Given the following position:

    [
      [ "s:r", "S:P", null, null, null, null ],
      [ null, null, null, null, null, null ],
      [ null, null, null, null, null, null ],
      [ null, null, null, null, null, null ],
      [ null, null, null, null, null, null ]
    ]

When this move, consisting of one action, is played:

    [[ 0, 1, "s:r" ]]

Then the position becomes:

    [
      [ null, "s:r", null, null, null, null ],
      [ null, null, null, null, null, null ],
      [ null, null, null, null, null, null ],
      [ null, null, null, null, null, null ],
      [ null, null, null, null, null, null ]
    ]

<span class="label label-info">Captured actors:</span>
The name of captured actors MUST change; e.g., at Shogi, the new side of captured actors can be deduced from the case.

#### Shift <small>an actor on the board</small>

Let's transfer a friendly actor (<q>Xiangqi Chariot, Black</q>) from its square to an _empty square_ of the board.

Given the following position:

    [
      [ "x:r", null, null, null, null, null ],
      [ null, null, null, null, null, null ],
      [ null, null, null, null, null, null ],
      [ null, null, null, null, null, null ],
      [ null, null, null, null, null, null ]
    ]

When this move is played:

    [[ 0, 6, "x:r" ]]

Then the position becomes:

    [
      [ null, null, null, null, null, null ],
      [ "x:r", null, null, null, null, null ],
      [ null, null, null, null, null, null ],
      [ null, null, null, null, null, null ],
      [ null, null, null, null, null, null ]
    ]

#### Promote <small>an actor</small>

Move to promote a Western <q>Pawn</q> to a <q>Queen</q>:

    [[ 15, 20, "w:q" ]]

Move to promote a Shogi <q>Pawn</q>:

    [[ 1, 2, "s:+p" ]]

### Indexes of board

The reserved "`indexes`" property is REQUIRED.

The "`indexes`" property specifies the shape of the board.

## Example

Here is the [<q>Immortal Game</q>](//en.wikipedia.org/wiki/Immortal_Game) in PCN format:

    {
      "started_at": "1851-06-21T16:00:00Z",

      "topside": {
        "name": "Lionel, Kieseritsky",
        "rating": 2700,
        "remaining_time": null
      },

      "bottomside": {
        "name": "Anderssen, Adolph",
        "rating": 2600,
        "remaining_time": null
      },

      "state": "checkmate",

      "indexes": [8, 8],

      "startpos": [
        "♜", "♞", "♝", "♛", "♚", "♝", "♞", "♜",
        "♟", "♟", "♟", "♟", "♟", "♟", "♟", "♟",
        null, null, null, null, null, null, null, null,
        null, null, null, null, null, null, null, null,
        null, null, null, null, null, null, null, null,
        null, null, null, null, null, null, null, null,
        "♙", "♙", "♙", "♙", "♙", "♙", "♙", "♙",
        "♖", "♘", "♗", "♕", "♔", "♗", "♘", "♖"
      ],

      "moves": [
        [[ 52, 36, "♙" ]],
        [[ 12, 28, "♟" ]],
        [[ 53, 37, "♙" ]],
        [[ 28, 37, "♟" ]],
        [[ 61, 34, "♗" ]],
        [[ 3, 39, "♛" ]],
        [[ 60, 61, "♔" ]],
        [[ 9, 25, "♟" ]],
        [[ 34, 25, "♗" ]],
        [[ 6, 21, "♞" ]],
        [[ 62, 45, "♘" ]],
        [[ 39, 23, "♛" ]],
        [[ 51, 43, "♙" ]],
        [[ 21, 31, "♞" ]],
        [[ 45, 39, "♘" ]],
        [[ 23, 30, "♛" ]],
        [[ 39, 29, "♘" ]],
        [[ 10, 18, "♟" ]],
        [[ 54, 38, "♙" ]],
        [[ 31, 21, "♞" ]],
        [[ 63, 62, "♖" ]],
        [[ 18, 25, "♟" ]],
        [[ 55, 39, "♙" ]],
        [[ 30, 22, "♛" ]],
        [[ 39, 31, "♙" ]],
        [[ 22, 30, "♛" ]],
        [[ 59, 45, "♕" ]],
        [[ 21, 6, "♞" ]],
        [[ 58, 37, "♗" ]],
        [[ 30, 21, "♛" ]],
        [[ 57, 42, "♘" ]],
        [[ 5, 26, "♝" ]],
        [[ 42, 27, "♘" ]],
        [[ 21, 49, "♛" ]],
        [[ 37, 19, "♗" ]],
        [[ 26, 62, "♝" ]],
        [[ 36, 28, "♙" ]],
        [[ 49, 56, "♛" ]],
        [[ 61, 52, "♔" ]],
        [[ 1, 16, "♞" ]],
        [[ 29, 14, "♘" ]],
        [[ 4, 3, "♚" ]],
        [[ 45, 21, "♕" ]],
        [[ 6, 21, "♞" ]],
        [[ 19, 12, "♗" ]]
      ]
    }

***

<!-- div class="sub-title">See also</div>

* [An implementation in Ruby](//github.com/sashite/pcn.rb) -->

<div class="sub-title">Informative References</div>

This work is influenced by several documents.

* [Portable Board Diff Notation](Portable-Board-Diff-Notation)
* [Portable Game Notation](//www.saremba.de/chessgml/standards/pgn/pgn-complete.htm)

<div class="sub-title">Contributing</div>

Want to make this page better?  [Make your changes](//github.com/sashite/specifications.md/edit/master/docs/Portable-Chess-Notation.md) and submit a hug request.
