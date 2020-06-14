# Portable Chess Notation

A general purpose JSON-based format for recording chess variants.

<dl class="dl-horizontal">
  <dt>Created</dt>
  <dd><time datetime="2012-08-05T01:23:45Z">August 5, 2012</time></dd>

  <dt>Updated</dt>
  <dd><time datetime="2020-06-14T17:29:55Z">June 14, 2020</time></dd>

  <dt>Version</dt>
  <dd>1.0.0</dd>
</dl>

<aside class="alert alert-warning">
  <strong>This is a work in progress!</strong>
  This is a beta document and may be updated at any time.
</aside>

***

<div class="sub-title">Abstract</div>

This document proposes a format for representing most **chess** games (both the _moves_ and related data).

<div class="sub-title">Status of this document</div>

This document is a beta of the PCN specification.

<div class="sub-title">Copyright notice</div>

Except as otherwise noted, the content of this page is licensed under the [Creative Commons Attribution 4.0 License](https://creativecommons.org/licenses/by/4.0/), and code samples are licensed under the [Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0).

***

## Introduction

<abbr title="Portable Chess Notation">PCN</abbr> (<q>Portable Chess Notation</q>) is a lightweight format based on <abbr title="JavaScript Object Notation">JSON</abbr> and <abbr title="Portable Move Notation">PMN</abbr> that gives a consistent and easy way to represent most chess games between two-players.

Able to describe both multidimensional games and related moves, in progress and finished games, easy for humans to read and write, and easy for machines to import and export, it is completely laws of chess independent and compatible with the main variants, including [<ruby lang="ko">장기<rt lang="en">Janggi</rt></ruby>](https://en.wikipedia.org/wiki/Janggi), [<ruby lang="th">หมากรุก<rt lang="en">Makruk</rt></ruby>](https://en.wikipedia.org/wiki/Makruk), [<ruby lang="ja">将棋<rt lang="en">Shogi</rt></ruby>](https://en.wikipedia.org/wiki/Shogi), [Chess](https://en.wikipedia.org/wiki/Chess), [<ruby lang="zh">象棋<rt lang="en">Xiangqi</rt></ruby>](https://en.wikipedia.org/wiki/Xiangqi).  These properties make PCN an ideal data-interchange format for recording chess games.

### Notational conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

## How to serve PCN

The use of the file suffix "`.pcn`" is RECOMMENDED for files containing PCN data.

When serving PCN over HTTP, the media type "`application/vnd.pcn+json`" is RECOMMENDED.

## <span id="resource">Notation for Games</span>

<div class="sub-title">Resource representation</div>

The JSON structure below shows the format of the resource:

    {
      "bottomside": {a hash representing a player},
      "event": {a string},
      "finished_at": {a datetime},
      "href": {a string},
      "indexes": {an array},
      "is_in_check": {a boolean},
      "is_topside_better": {a boolean},
      "is_turn_to_topside": {a boolean},
      "location": {a string},
      "moves": {an array},
      "name": {a string},
      "round": {an unsigned integer},
      "started_on": {a date},
      "startpos": {an array},
      "topside": {a hash representing a player}
    }

<div class="sub-title">Properties</div>

The following table defines the properties that appear in this resource:

| Property name                                       | Value                                         | Required  | Default | Description |
| --------------------------------------------------- | --------------------------------------------- | --------- | ------- | ----------- |
| [`bottomside`](#property-bottomside)                | a hash representing a [player](#value-player) | false     | `{}`    | The bottom-side player. |
| [`event`](#property-event)                          | a string                                      | false     | `null`  | Indicates the name of the tournament or match event. |
| [`finished_at`](#property-finished_at)              | a datetime                                    | false     | `null`  | Indicates the date and time when the game should end or ended. |
| [`href`](#property-href)                            | a string                                      | false     | `null`  | Indicates a website that display the game. |
| [`indexes`](#property-indexes)                      | an array                                      | true      |         | The shape of the board. |
| [`is_in_check`](#property-is_in_check)              | a boolean                                     | false     |         | Indicates the King (or the General in Xiangqi) is under threat of capture on their opponent's next turn. |
| [`is_topside_better`](#property-is_topside_better)  | a boolean                                     | false     | `null`  | When the game is in progress, indicates whether top-side would win, lose or if there would be a draw at the end of regulation time.  Or when the game is over, indicate if top-side has won, lost or if there is a draw. |
| [`is_turn_to_topside`](#property-is_turn_to_topside) | a boolean                                    | false     |         | Indicates the player who must play. |
| [`location`](#property-location)                    | a string                                      | false     | `null`  | Indicates the place or position that the game was in or where it happened. |
| [`moves`](#property-moves)                          | an array                                      | false     | `[]`    | Indicates the moves previously played. |
| [`name`](#property-name)                            | a string                                      | false     | `null`  | Indicates the name of the game. |
| [`round`](#property-round)                          | an unsigned integer                           | false     | `null`  | Indicates the playing round ordinal of the game. |
| [`started_on`](#property-started_on)                | a date                                        | false     | `null`  | The date that the game was started. |
| [`startpos`](#property-startpos)                    | an array                                      | false     | `null` | List of squares that identify pieces on the starting position. |
| [`topside`](#property-topside)                      | a hash representing a [player](#value-player) | false     | `{}`    | The top-side player. |


<h3 id="property-bottomside">Bottom-side player</h3>

The reserved "`bottomside`" property is RECOMMENDED.

The "`bottomside`" property represents a [player](#value-player).


<h3 id="property-event">Event</h3>

The reserved "`event`" property is RECOMMENDED.

The "`event`" property is the name of the tournament or match event.


<h3 id="property-finished_at">Finished datetime</h3>

The reserved "`finished_at`" property is RECOMMENDED.

The "`finished_at`" property specifies the date and time when the game should end or ended.

The value is specified in [ISO 8601](https://www.w3.org/TR/NOTE-datetime) format (eg "`1994-11-05T13:15:30Z`").

If the property is not present, or if the property is present and its value is `null` or greater than or equal to the current datetime, the game MUST be considered finished.
Otherwise, the game MUST be considered in progress because the value is less than the current datetime.


<h3 id="property-href">Hyperlink</h3>

The reserved "`href`" property is RECOMMENDED.

The "`href`" property specifies a link that targets a document about the game.


<h3 id="property-indexes">Indexes of board</h3>

The reserved "`indexes`" property is REQUIRED.

The "`indexes`" property specifies the shape of the board.

This is a tuple of integers indicating the size of the array in each dimension.  For a two-dimensional board, `n` would be the number of rows and `m` the number of columns: `[n, m]`.

For instance, for the Western chessboard the "`indexes`" value MUST be: `[8, 8]`.  But for the Xiangqi chessboard the "`indexes`" value MUST be: `[10, 9]`.


<h3 id="property-is_in_check">In check?</h3>

The reserved "`is_in_check`" property is RECOMMENDED.

The "`is_in_check`" property specifies a player's king (or general in xiangqi and janggi) is under threat of capture on their opponent's next turn.


<h3 id="property-is_topside_better">Top-side better?</h3>

The reserved "`is_topside_better`" property is RECOMMENDED.

When the game is in progress, indicates whether top-side would win, lose or if there would be a draw at the end of regulation time.
Or when the game is over, indicates if top-side has won, lost or if there is a draw.


<h3 id="property-is_turn_to_topside">Turn to top-side?</h3>

The reserved "`is_turn_to_topside`" property is RECOMMENDED.

Indicates the player who must play (the current player).


<h3 id="property-location">Location</h3>

The reserved "`location`" property is RECOMMENDED.

Indicates the place or position that the game was in or where it happened.


<h3 id="property-moves">Moves on the chessboard</h3>

The reserved "`moves`" property is OPTIONAL.

The moves are an ordered set of _action_ based on [Portable Move Notation](https://developer.sashite.com/specs/portable-move-notation) format.  A move MUST contain at least one action.

For example, the following move:

    [ 42, 43, "+R", null ]

can be translated into:

> Move a piece from coordinate 42 to coordinate 43 which becomes a <q>Shogi promoted Rook</q>.

<p class="alert alert-info">
  Depending of the context of the board, this move MAY remove from the board a piece at coordinate 43.
</p>

If the property is not present, or if the property is present and its value is `null`, the list of moves MUST be considered as empty.


<h3 id="property-name">Name</h3>

The reserved "`name`" property is RECOMMENDED.

Indicates the name of the game.


<h3 id="property-round">Round</h3>

The reserved "`round`" property is RECOMMENDED.

The "`round`" property is the number of the game played between two players.

If present, the value MUST be greater than or equal to `1`.


<h3 id="property-started_on">Started date</h3>

The reserved "`started_on`" property is RECOMMENDED.

The "`started_on`" property specifies the starting date for the game.

The value is specified in [ISO 8601](https://www.w3.org/TR/NOTE-datetime) format (eg "`1997-07-16`").


<h3 id="property-startpos">Starting position</h3>

The reserved "`startpos`" property is RECOMMENDED.

The "`startpos`" property specifies a one-dimensional list of squares representing the starting position.

If the property is not present, or if the property is present and its value is `null`, each square of the board MUST be considered as empty.

Janggi starting position example:

    [
      "r", "m", "e", "s", null, "s", "e", "m", "r",
      null, null, null, null, "g", null, null, null, null,
      null, "p", null, null, null, null, null, "p", null,
      "j", null, "j", null, "j", null, "j", null, "j",
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      "J", null, "J", null, "J", null, "J", null, "J",
      null, "P", null, null, null, null, null, "P", null,
      null, null, null, null, "G", null, null, null, null,
      "R", "M", "E", "S", null, "S", "E", "M", "R"
    ]

Makruk starting position example:

    [
      "r", "n", "b", "q", "k", "b", "n", "r",
      null, null, null, null, null, null, null, null,
      "p", "p", "p", "p", "p", "p", "p", "p",
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      "P", "P", "P", "P", "P", "P", "P", "P",
      null, null, null, null, null, null, null, null,
      "R", "N", "B", "K", "Q", "B", "N", "R"
    ]

Shogi starting position example:

    [
      "l", "n", "s", "g", "k", "g", "s", "n", "l",
      null, "r", null, null, null, null, null, "b", null,
      "p", "p", "p", "p", "p", "p", "p", "p", "p",
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      "P", "P", "P", "P", "P", "P", "P", "P", "P",
      null, "B", null, null, null, null, null, "R", null,
      "L", "N", "S", "G", "K", "G", "S", "N", "L"
    ]

Western starting position example:

    [
      "♜", "♞", "♝", "♛", "♚", "♝", "♞", "♜",
      "♟", "♟", "♟", "♟", "♟", "♟", "♟", "♟",
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      "♙", "♙", "♙", "♙", "♙", "♙", "♙", "♙",
      "♖", "♘", "♗", "♕", "♔", "♗", "♘", "♖"
    ]

Xiangqi starting position example:

    [
      "車", "馬", "象", "士", "將", "士", "象", "馬", "車",
      null, null, null, null, null, null, null, null, null,
      null, "砲", null, null, null, null, null, "砲", null,
      "卒", null, "卒", null, "卒", null, "卒", null, "卒",
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      "兵", null, "兵", null, "兵", null, "兵", null, "兵",
      null, "炮", null, null, null, null, null, "炮", null,
      null, null, null, null, null, null, null, null, null,
      "俥", "傌", "相", "仕", "帥", "仕", "相", "傌", "俥"
    ]


<h3 id="property-topside">Top-side player</h3>

The reserved "`topside`" property is RECOMMENDED.

The "`topside`" property represents a [player](#value-player).


<h3 id="value-player">Player</h3>

<div class="sub-title">Resource representation</div>

The JSON structure below shows the format of the resource:

    {
      "elo": {an unsigned integer},
      "in_hand_pieces": {an array},
      "is_requesting_a_draw": {a boolean},
      "name": {a string}
    }

<div class="sub-title">Properties</div>

The following table defines the properties that appear in this resource:

| Property name                                                   | Value               | Required  | Default | Description |
| --------------------------------------------------------------- | ------------------- | --------- | ------- | ----------- |
| [`elo`](#player-property-elo)                                   | an unsigned integer | false     | `null`  | The Elo rating of the player. |
| [`in_hand_pieces`](#player-property-in_hand_pieces)             | an array            | false     | `[]`    | The list of pieces in hand. |
| [`is_requesting_a_draw`](#player-property-is_requesting_a_draw) | a boolean           | false     | `false` | The player is offering a draw. |
| [`name`](#player-property-name)                                 | a string            | false     | `null`  | The name of the player. |

<h4 id="player-property-elo">Player Elo</h4>

The reserved "`elo`" property is RECOMMENDED.

The "`elo`" property represents the relative skill level of a player at the beginning of the game, according to the Elo rating system.

If present, the value MUST be greater than or equal to `0`.

<h4 id="player-property-in_hand_pieces">Pieces in hand</h4>

The reserved "`in_hand_pieces`" property is RECOMMENDED.

The "`in_hand_pieces`" property represents the relative skill level of a player at the beginning of the game, according to the Elo rating system.

If the property is not present, or if the property is present and its value is `null`, the list of pieces in hand MUST be considered as empty.

<h4 id="player-property-is_requesting_a_draw">Draw request</h4>

The reserved "`is_requesting_a_draw`" property is RECOMMENDED.

The "`is_requesting_a_draw`" property represents a proposal for a draw from the player to the opponent player.
If the opponent accepts, the game is a draw (by mutual agreement).

If the property is not present, or if the property is present and its value is `null`, there is no proposal for a draw.

<h4 id="player-property-name">Player's name</h4>

The reserved "`name`" property is RECOMMENDED.

The "`name`" property represents the name of the player.

## Chess game example

The [<q>Immortal Game</q>](https://en.wikipedia.org/wiki/Immortal_Game) in PCN format:

    {
      "name": "Immortal Game",

      "event": "London",
      "location": "London ENG",
      "href": "https://en.wikipedia.org/wiki/Immortal_Game",

      "started_on": "1851-06-21",

      "is_in_check": true,
      "is_topside_better": false,

      "topside": {
        "name": "Lionel Adalbert Bagration Felix Kieseritzky"
      },

      "bottomside": {
        "name": "Adolf Anderssen"
      },

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

## Shogi game example

[The shortest possible game](https://userpages.monmouth.com/~colonel/shortshogi.html) in PCN format:

    {
      "name": "The Shortest Possible Game of Shogi",

      "is_in_check": true,
      "is_topside_better": false,

      "href": "https://userpages.monmouth.com/~colonel/shortshogi.html",

      "indexes": [9, 9],

      "startpos": [
        "l", "n", "s", "g", "k", "g", "s", "n", "l",
        null, "r", null, null, null, null, null, "b", null,
        "p", "p", "p", "p", "p", "p", "p", "p", "p",
        null, null, null, null, null, null, null, null, null,
        null, null, null, null, null, null, null, null, null,
        null, null, null, null, null, null, null, null, null,
        "P", "P", "P", "P", "P", "P", "P", "P", "P",
        null, "B", null, null, null, null, null, "R", null,
        "L", "N", "S", "G", "K", "G", "S", "N", "L"
      ],

      "moves": [
        [ 56, 47, "P" ],
        [ 3, 11, "g" ],
        [ 64, 24, "+B", "P" ],
        [ 5, 14, "g" ],
        [ 24, 14, "+B", "G" ],
        [ 4, 3, "k" ],
        [ null, 13, "G" ]
      ]
    }

## Xiangqi game example

A very short game in PCN format:

    {
      "name": "Short Double Cannons Checkmate",

      "is_in_check": true,
      "is_topside_better": false,

      "indexes": [10, 9],

      "startpos": [
        "車", "馬", "象", "士", "將", "士", "象", "馬", "車",
        null, null, null, null, null, null, null, null, null,
        null, "砲", null, null, null, null, null, "砲", null,
        "卒", null, "卒", null, "卒", null, "卒", null, "卒",
        null, null, null, null, null, null, null, null, null,
        null, null, null, null, null, null, null, null, null,
        "兵", null, "兵", null, "兵", null, "兵", null, "兵",
        null, "炮", null, null, null, null, null, "炮", null,
        null, null, null, null, null, null, null, null, null,
        "俥", "傌", "相", "仕", "帥", "仕", "相", "傌", "俥"
      ],

      "moves": [
        [ 64, 67, "炮" ],
        [ 25, 22, "砲" ],
        [ 70, 52, "炮" ],
        [ 19, 55, "砲" ],
        [ 67, 31, "炮" ],
        [ 22, 58, "砲" ],
        [ 52, 49, "炮" ]
      ]
    }

***

<div class="sub-title">See also</div>

* [Portable Move Notation](https://developer.sashite.com/specs/portable-move-notation)

<div class="sub-title">Implementation</div>

* [A PCN viewer in JavaScript](https://github.com/sashite/pcn-viewer.js)

<div class="sub-title">Informative References</div>

This work is influenced by several documents.

* [Portable Game Notation](https://www.chessclub.com/help/PGN-spec)

<div class="sub-title">Contributing</div>

Want to make this page better?  [Make your changes](https://github.com/sashite/specifications.md/edit/master/docs/Portable-Chess-Notation.md) and submit a hug request.
