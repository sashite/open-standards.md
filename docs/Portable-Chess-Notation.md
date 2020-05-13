# Portable Chess Notation

A general purpose JSON-based format for recording chess variants.

<dl class="dl-horizontal">
  <dt>Created</dt>
  <dd><time datetime="2012-08-05T01:23:45Z">August 5, 2012</time></dd>

  <dt>Updated</dt>
  <dd><time datetime="2020-05-11T17:29:55Z">May 11, 2020</time></dd>

  <dt>Version</dt>
  <dd>0.1.0</dd>
</dl>

<div class="alert alert-warning">
  <strong>This is a work in progress!</strong>
  This is a beta document and may be updated at any time.
</div>

***

<div class="sub-title">Abstract</div>

This document proposes a format for representing most **chess** games (both the _moves_ and related data).

<div class="sub-title">Status of this document</div>

This document is a beta of the PCN specification.

<div class="sub-title">Copyright notice</div>

Except as otherwise noted, the content of this page is licensed under the [Creative Commons Attribution 4.0 License](https://creativecommons.org/licenses/by/4.0/), and code samples are licensed under the [Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0).

***

## Introduction

<abbr title="Portable Chess Notation">PCN</abbr> (<q>Portable Chess Notation</q>) is a lightweight format based on <abbr title="JavaScript Object Notation">JSON</abbr> and <abbr title="Portable Board Diff Notation">PBDN</abbr> that gives a consistent and easy way to represent most chess games between two-players.

Able to describe both multidimensional starting positions and related moves, easy for humans to read and write, and easy for machines to import and export, it is completely laws of chess independent and compatible with the main variants, including [<ruby lang="ko">장기<rt lang="en">Janggi</rt></ruby>](https://en.wikipedia.org/wiki/Janggi), [<ruby lang="th">หมากรุก<rt lang="en">Makruk</rt></ruby>](https://en.wikipedia.org/wiki/Makruk), [<ruby lang="ja">将棋<rt lang="en">Shogi</rt></ruby>](https://en.wikipedia.org/wiki/Shogi), [Western](https://en.wikipedia.org/wiki/Chess), [<ruby lang="zh">象棋<rt lang="en">Xiangqi</rt></ruby>](https://en.wikipedia.org/wiki/Xiangqi).  These properties make PCN an ideal data-interchange format for recording chess games.

### Notational conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

#### Naming the actors

The key word "actor" and its notation are to be interpreted as described in [General Actor Notation](https://developer.sashite.com/specs/general-actor-notation).

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
      "is_topside_better": {a boolean},
      "is_in_check": {a boolean},
      "event": {a string},
      "location": {a string},
      "round": {an integer},
      "started_on": {a date},
      "finished_at": {a datetime},
      "topside": {a hash representing a player},
      "bottomside": {a hash representing a player},
      "indexes": {an array},
      "startpos": {an array},
      "moves": {an array}
    }

<div class="sub-title">Properties</div>

The following table defines the properties that appear in this resource:

| Property name   | Value      | Description |
| --------------- | ---------- | ----------- |
| "`is_topside_better`" | a boolean | In the current state of the game, when the game is in progress, indicates whether topside would win, lose or if there would be a draw at the end of regulation time.  Or when the game is over, indicate if topside has won, lost or if there is a draw. |
| "`is_in_check`" | a boolean | Indicates a player's King (or General in Xiangqi and Janggi) is under threat of capture on their opponent's next turn. |
| "`event`"       | a string   | Indicates the name of the tournament or match event. |
| "`location`"    | a string   | Indicates the place or position that the game was in or where it happened. |
| "`round`"       | a integer  | Indicates the playing round ordinal of the game. |
| "`started_on`"  | a date     | The date that the game was started. |
| "`finished_at`" | a datetime | Indicates the date and time when the game should end or ended. |
| "`topside`"     | a hash representing a player | The topside player. |
| "`bottomside`"  | a hash representing a player | The bottomside player. |
| "`indexes`"     | an array   | The shape of the board. |
| "`startpos`"    | an array   | List of squares that identify actors on the board's starting position. |
| "`moves`"       | an array   | List of moves associated with the game. |

### Top-side & Bottom-side

The reserved "`topside`" & "`bottomside`" properties are RECOMMENDED.

<div class="sub-title">Properties</div>

The following table defines the properties that appear in this resource:

| Property name            | Value               | Description |
| ------------------------ | ------------------- | ----------- |
| "`name`"                 | a string            | The name of the player. |
| "`is_requesting_a_draw`" | a boolean           | The player is offering a draw. |
| "`elo`"                  | an unsigned integer | The Elo rating of the player. |

### Round

The reserved "`round`" property is RECOMMENDED.

The "`round`" property is the number of the game played between two players.

If present, the value MUST be greater than or equal to `1`.

### Started date

The reserved "`started_on`" property is RECOMMENDED.

The "`started_on`" property specifies the starting date for the game.

The value is specified in [ISO 8601](https://www.w3.org/TR/NOTE-datetime) format (eg "`1997-07-16`").

### Finished datetime

The reserved "`finished_at`" property is RECOMMENDED.

The "`finished_at`" property specifies the date and time when the game should end or ended.

The value is specified in [ISO 8601](https://www.w3.org/TR/NOTE-datetime) format (eg "`1994-11-05T13:15:30Z`").

If the property is not present, or if the property is present and its value is `null` or greater than or equal to the current datetime, the game MUST be considered finished.
Otherwise, the game MUST be considered in progress because the value is less than the current datetime.

### Indexes of board

The reserved "`indexes`" property is REQUIRED.

The "`indexes`" property specifies the shape of the board.

This is a tuple of integers indicating the size of the array in each dimension.  For a two-dimensional board, `n` would be the number of rows and `m` the number of columns: `[n, m]`.

For instance, for the Western chessboard the "`indexes`" value MUST be: `[8, 8]`.  But for the Xiangqi chessboard the "`indexes`" value MUST be: `[10, 9]`.

### Starting position

The reserved "`startpos`" property is REQUIRED.

Chess positions on the chessboard is represented by an array.

#### Janggi chessboard

    [
      "j:r", "j:m", "j:e", "j:s", null, "j:s", "j:e", "j:m", "j:r",
      null, null, null, null, "j:-g", null, null, null, null,
      null, "j:p", null, null, null, null, null, "j:p", null,
      "j:j", null, "j:j", null, "j:j", null, "j:j", null, "j:j",
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      "J:J", null, "J:J", null, "J:J", null, "J:J", null, "J:J",
      null, "J:P", null, null, null, null, null, "J:P", null,
      null, null, null, null, "J:-G", null, null, null, null,
      "J:R", "J:M", "J:E", "J:S", null, "J:S", "J:E", "J:M", "J:R"
    ]

#### Makruk chessboard

    [
      "m:r", "m:n", "m:b", "m:q", "m:-k", "m:b", "m:n", "m:r",
      null, null, null, null, null, null, null, null,
      "m:p", "m:p", "m:p", "m:p", "m:p", "m:p", "m:p", "m:p",
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      "M:P", "M:P", "M:P", "M:P", "M:P", "M:P", "M:P", "M:P",
      null, null, null, null, null, null, null, null,
      "M:R", "M:N", "M:B", "M:-K", "M:Q", "M:B", "M:N", "M:R"
    ]

#### Shogi chessboard

    [
      "s:l", "s:n", "s:s", "s:g", "s:-k", "s:g", "s:s", "s:n", "s:l",
      null, "s:r", null, null, null, null, null, "s:b", null,
      "s:p", "s:p", "s:p", "s:p", "s:p", "s:p", "s:p", "s:p", "s:p",
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      "S:P", "S:P", "S:P", "S:P", "S:P", "S:P", "S:P", "S:P", "S:P",
      null, "S:B", null, null, null, null, null, "S:R", null,
      "S:L", "S:N", "S:S", "S:G", "S:-K", "S:G", "S:S", "S:N", "S:L"
    ]

#### Western chessboard

    [
      "w:r", "w:n", "w:b", "w:q", "w:-k", "w:b", "w:n", "w:r",
      "w:p", "w:p", "w:p", "w:p", "w:p", "w:p", "w:p", "w:p",
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      "W:P", "W:P", "W:P", "W:P", "W:P", "W:P", "W:P", "W:P",
      "W:R", "W:N", "W:B", "W:Q", "W:-K", "W:B", "W:N", "W:R"
    ]

#### Xiangqi chessboard

    [
      "x:r", "x:h", "x:e", "x:a", "x:-g", "x:a", "x:e", "x:h", "x:r",
      null, null, null, null, null, null, null, null, null,
      null, "x:c", null, null, null, null, null, "x:c", null,
      "x:s", null, "x:s", null, "x:s", null, "x:s", null, "x:s",
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      "X:S", null, "X:S", null, "X:S", null, "X:S", null, "X:S",
      null, "X:C", null, null, null, null, null, "X:C", null,
      null, null, null, null, null, null, null, null, null,
      "X:R", "X:H", "X:E", "X:A", "X:-G", "X:A", "X:E", "X:H", "X:R"
    ]

### Moves on the chessboard

The reserved "`moves`" property is REQUIRED.

The moves are an ordered set of _action_ expressed into [Portable Board Diff Notation](https://developer.sashite.com/specs/portable-board-diff-notation).  A move MUST contain one action.

For example, the following move:

    [ 42, 43, "S:+R" ]

can be translated into:

> Move a piece from coordinate 42 to coordinate 43 which becomes a <q>Shogi promoted Rook</q>.

<div class="alert alert-info">
  Depending of the context of the board, this move MAY remove from the board a piece at coordinate 43.
</div>

<div class="sub-title">Several scenarios</div>

#### Drop

Captured actors can be truly _captured_, such as in Shogi.  Thus, they are retained <q>in hand</q>, and can be brought back into play under the capturing player's control.  On any turn, instead of moving an actor on the board, a player MAY take an actor that had been previously captured and place it.  The actor is then part of the forces controlled by that player.  This is termed _dropping_ the actor, or just a _drop_.

Given the following position:

    [
      null, null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null
    ]

When this move is played:

    [ null, 2, "S:R" ]

Then the position becomes:

    [
      null, null, "S:R", null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null
    ]

#### Capture

Let's detach an opponent's actor from the board by taking it with a friendly actor:

Given the following position:

    [
      "s:r", "S:P", null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null
    ]

When this move is played:

    [ 0, 1, "s:r", "s:p" ]

Then the position becomes:

    [
      null, "s:r", null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null
    ]

Note: The 4th parameter indicates that the captured "`S:P`" piece becomes "`s:p`" and is retained in hand by the player who played.

#### Shift

Let's transfer a friendly actor (<q>Xiangqi Chariot, Black</q>) from its square to an _empty square_ of the board.

Given the following position:

    [
      "x:r", null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null
    ]

When this move is played:

    [ 0, 6, "x:r" ]

Then the position becomes:

    [
      null, null, null, null, null, null,
      "x:r", null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null
    ]

#### Promotion

##### Western <q>Pawn</q> to a <q>Queen</q>

Given the following position:

    [
      null, null, null, null, null, null,
      "W:P", null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null
    ]

When this move is played:

    [ 6, 0, "W:Q" ]

Then the position becomes:

    [
      "W:Q", null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null
    ]

##### Shogi <q>Pawn</q> to a promoted Shogi <q>Pawn</q>

Given the following position:

    [
      null, null, null, null, null, null,
      "S:P", null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null
    ]

When this move is played:

    [ 6, 0, "S:+P" ]

Then the position becomes:

    [
      "S:+P", null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null,
      null, null, null, null, null, null
    ]

## Example

Here is the [<q>Immortal Game</q>](https://en.wikipedia.org/wiki/Immortal_Game) in PCN format:

    {
      "event": "London",
      "location": "London ENG",

      "started_on": "1851-06-21",

      "is_in_check": true,
      "is_topside_better": false,

      "topside": { "name": "Lionel Adalbert Bagration Felix Kieseritzky" },
      "bottomside": { "name": "Adolf Anderssen" },

      "state": "bottomside_won",

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
        [ 52, 36, "♙" ],
        [ 12, 28, "♟" ],
        [ 53, 37, "♙" ],
        [ 28, 37, "♟" ],
        [ 61, 34, "♗" ],
        [ 3, 39, "♛" ],
        [ 60, 61, "♔" ],
        [ 9, 25, "♟" ],
        [ 34, 25, "♗" ],
        [ 6, 21, "♞" ],
        [ 62, 45, "♘" ],
        [ 39, 23, "♛" ],
        [ 51, 43, "♙" ],
        [ 21, 31, "♞" ],
        [ 45, 39, "♘" ],
        [ 23, 30, "♛" ],
        [ 39, 29, "♘" ],
        [ 10, 18, "♟" ],
        [ 54, 38, "♙" ],
        [ 31, 21, "♞" ],
        [ 63, 62, "♖" ],
        [ 18, 25, "♟" ],
        [ 55, 39, "♙" ],
        [ 30, 22, "♛" ],
        [ 39, 31, "♙" ],
        [ 22, 30, "♛" ],
        [ 59, 45, "♕" ],
        [ 21, 6, "♞" ],
        [ 58, 37, "♗" ],
        [ 30, 21, "♛" ],
        [ 57, 42, "♘" ],
        [ 5, 26, "♝" ],
        [ 42, 27, "♘" ],
        [ 21, 49, "♛" ],
        [ 37, 19, "♗" ],
        [ 26, 62, "♝" ],
        [ 36, 28, "♙" ],
        [ 49, 56, "♛" ],
        [ 61, 52, "♔" ],
        [ 1, 16, "♞" ],
        [ 29, 14, "♘" ],
        [ 4, 3, "♚" ],
        [ 45, 21, "♕" ],
        [ 6, 21, "♞" ],
        [ 19, 12, "♗" ]
      ]
    }

***

<!-- div class="sub-title">See also</div>

* [An implementation in Ruby](https://github.com/sashite/pcn.rb) -->

<div class="sub-title">Informative References</div>

This work is influenced by several documents.

* [Portable Board Diff Notation](https://developer.sashite.com/specs/portable-board-diff-notation)
* [Portable Game Notation](https://www.chessclub.com/help/PGN-spec)

<div class="sub-title">Contributing</div>

Want to make this page better?  [Make your changes](https://github.com/sashite/specifications.md/edit/master/docs/Portable-Chess-Notation.md) and submit a hug request.
