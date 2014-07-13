# Portable Chess Notation <small>Specification and Implementation Guide</small>

A general purpose JSON-based format for recording chess variants' games.

<dl class="dl-horizontal">
  <dt>Created</dt>
  <dd><time datetime="2012-08-05T01:23:45Z">5 August 2012</time></dd>

  <dt>Updated</dt>
  <dd><time datetime="2014-07-11T23:42:34Z">11 July 2014</time></dd>

  <dt>Status</dt>
  <dd>beta</dd>

  <dt>Author</dt>
  <dd><a rel="external author" href="https://plus.google.com/+CyrilWack">Cyril Wack</a></dd>
</dl>

<div class="alert alert-warning">
  <button type="button" class="close" data-dismiss="alert">&times;</button>
  <strong>This is a work in progress!</strong>
  This is a beta document and may be updated at any time.
</div>

* * *

<div class="sub-title">Abstract</div>

This document proposes a format for representing most **chess** games (both the _moves_ and related data).

<div class="sub-title">Status of this document</div>

This document is a beta of the PCN specification.

<div class="sub-title">Copyright Notice</div>

The content of this page is licensed under the [Creative Commons Attribution 3.0 License](//creativecommons.org/licenses/by/3.0/), and code samples are licensed under the [Apache 2.0 License](//www.apache.org/licenses/LICENSE-2.0).

* * *

## Introduction

<abbr title="Portable Chess Notation">PCN</abbr> (<q>Portable Chess Notation</q>) is a lightweight format based on <abbr title="JavaScript Object Notation">JSON</abbr> and <abbr title="Portable Action Notation">PAN</abbr> that gives a consistent and easy way to represent most chess games between two-players.

Able to describe both multidimensional starting positions and related moves, easy for humans to read and write, and easy for machines to import and export, it is completely laws of chess independent and compatible with the main variants, including [<ruby lang="ko">장기<rt lang="en">Janggi</rt></ruby>](//en.wikipedia.org/wiki/Janggi), [<ruby lang="th">หมากรุก<rt lang="en">Makruk</rt></ruby>](//en.wikipedia.org/wiki/Makruk), [<ruby lang="ja">将棋<rt lang="en">Shogi</rt></ruby>](//en.wikipedia.org/wiki/Shogi), [Western](//en.wikipedia.org/wiki/Chess), [<ruby lang="zh">象棋<rt lang="en">Xiangqi</rt></ruby>](//en.wikipedia.org/wiki/Xiangqi).  These properties make PCN an ideal data-interchange format for recording chess games.

### Notational conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](//tools.ietf.org/html/rfc2119).

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
      "side_bottom_name": {a string},
      "side_bottom_rating°": {an unsigned integer},
      "side_bottom_draw_offer?": {a boolean},
      "side_top_name": {a string},
      "side_top_rating°": {an unsigned integer},
      "side_top_draw_offer?": {a boolean},
      "state": {a string},
      "started_at": {a datetime},
      "startpos": {an array},
      "moves": {an array},
      "version": {a string}
    }

<div class="sub-title">Properties</div>

The following table defines the properties that appear in this resource:

| Property name   | Value                | Description |
| --------------- | -------------------- | ----------- |
| "`side_bottom_name`"        | a string            | The name of bottom. |
| "`side_bottom_rating°`"     | an unsigned integer | The rating of bottom. |
| "`side_bottom_draw_offer?`" | a boolean           | The draw offer request of bottom. |
| "`side_top_name`"           | a string            | The name of top. |
| "`side_top_rating°`"        | an unsigned integer | The rating of top. |
| "`side_top_draw_offer?`"    | a boolean           | The draw offer request of top. |
| "`state`"       | a string             | Indicates the state of the game. |
| "`started_at`"  | a datetime           | The date and time that the game was started.  The value is specified in [ISO 8601](//www.w3.org/TR/NOTE-datetime) format. |
| "`startpos`"    | an array             | List of squares that identify actors on the board's starting position. |
| "`moves`"       | an array             | List of moves associated with the game. |
| "`version`"     | a string             | Version number of the current PCN document.  The value is specified in [Semantic Versioning 2.0.0](//semver.org/spec/v2.0.0.html) format. |

### Bottom-side's name

The reserved "`side_bottom_name`" property is RECOMMENDED.

As in [GAN](General-Actor-Notation)'s <q>Naming the sides</q>, _the player who moves first_ is referred to as **bottom**.

### Bottom-side's rating

The reserved "`side_bottom_rating°`" property is RECOMMENDED.

The rating system MAY be Elo.

### Bottom-side's draw offer request

The reserved "`side_bottom_draw_offer?`" property is RECOMMENDED.

### Top-side's name

The reserved "`side_top_name`" property is RECOMMENDED.

As in [GAN](General-Actor-Notation)'s <q>Naming the sides</q>, _the player who moves second_ is referred to as **top**.

### Top-side's rating

The reserved "`side_top_rating°`" property is RECOMMENDED.

The rating system MAY be Elo.

### Top-side's draw offer request

The reserved "`side_top_draw_offer?`" property is RECOMMENDED.

### State

The reserved "`state`" property is REQUIRED.

The possible values are:

* "`in_progress`"
* "`checkmate`"
* "`abandoned`"
* "`time_forfeit`"
* "`drawn_game`"

### Starting datetime

The reserved "`started_at`" property is RECOMMENDED.

The "`started_at`" property specifies the starting datetime for the game.

The value is specified in [ISO 8601](//www.w3.org/TR/NOTE-datetime) format.

### Starting position

The reserved "`startpos`" property is REQUIRED.

#### Board

Chess positions on the chessboard can be represented by an array.

Even though it is unusual, it is possible to represent a chessboard from one dimension.  Each dimension can be described by an embedded array (where the first dimension SHOULD be the root array).

<div class="panel panel-primary">
  <div class="panel-heading">
    <span class="panel-title">Empty chessboard examples</span>
  </div>

  <div class="panel-body">
    <ul class="nav nav-tabs">
      <li class="active">
        <a href="#board-empty_chessboard_examples-6_size" data-toggle="tab">6 size</a>
      </li>

      <li>
        <a href="#board-empty_chessboard_examples-4x8_size" data-toggle="tab">4x8 size</a>
      </li>

      <li>
        <a href="#board-empty_chessboard_examples-7x7_size" data-toggle="tab">7x7 size</a>
      </li>

      <li>
        <a href="#board-empty_chessboard_examples-5x5x5_size" data-toggle="tab">5x5x5 size</a>
      </li>
    </ul>

    <div class="tab-content">
      <div class="tab-pane fade active in" id="board-empty_chessboard_examples-6_size">
        <pre><code class="json">[null, null, null, null, null, null]</code></pre>
      </div>

      <div class="tab-pane fade" id="board-empty_chessboard_examples-4x8_size">
        <pre><code class="json">[
  [null, null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null, null]
]</code></pre>

        <blockquote>
          <p>
            Used in the game:
            <cite title="Wikipedia"><a rel="external" href="//en.wikipedia.org/wiki/Banqi">Banqi (<span lang="zh">盲棋</span>)</a></cite>.
          </p>
        </blockquote>
      </div>

      <div class="tab-pane fade" id="board-empty_chessboard_examples-7x7_size">
        <pre><code class="json">[
  [null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null]
]</code></pre>

        <blockquote>
          <p>
            Used in the game:
            <cite title="Wikipedia"><a rel="external" href="//en.wikipedia.org/wiki/Tori_shogi">Tori Shogi (<span lang="ja">禽将棋</span>)</a></cite>.
          </p>
        </blockquote>
      </div>

      <div class="tab-pane fade" id="board-empty_chessboard_examples-5x5x5_size">
        <pre><code class="json">[
  [
    [null, null, null, null, null],
    [null, null, null, null, null],
    [null, null, null, null, null],
    [null, null, null, null, null],
    [null, null, null, null, null]
  ],
  [
    [null, null, null, null, null],
    [null, null, null, null, null],
    [null, null, null, null, null],
    [null, null, null, null, null],
    [null, null, null, null, null]
  ],
  [
    [null, null, null, null, null],
    [null, null, null, null, null],
    [null, null, null, null, null],
    [null, null, null, null, null],
    [null, null, null, null, null]
  ],
  [
    [null, null, null, null, null],
    [null, null, null, null, null],
    [null, null, null, null, null],
    [null, null, null, null, null],
    [null, null, null, null, null]
  ],
  [
    [null, null, null, null, null],
    [null, null, null, null, null],
    [null, null, null, null, null],
    [null, null, null, null, null],
    [null, null, null, null, null]
  ]
]</code></pre>

        <blockquote>
          <p>
            Used in the game:
            <cite title="Wikipedia"><a rel="external" href="//en.wikipedia.org/wiki/Three-dimensional_chess#Raumschach">Raumschach</a></cite>.
          </p>
        </blockquote>
      </div>
    </div>
  </div>
</div>

#### Examples

<div class="panel-body">
  <ul class="nav nav-tabs">
    <li class="active">
      <a href="#board-startpos_examples-janggi" data-toggle="tab">Janggi</a>
    </li>

    <li>
      <a href="#board-startpos_examples-makruk" data-toggle="tab">Makruk</a>
    </li>

    <li>
      <a href="#board-startpos_examples-shogi" data-toggle="tab">Shogi</a>
    </li>

    <li>
      <a href="#board-startpos_examples-western" data-toggle="tab">Western</a>
    </li>

    <li>
      <a href="#board-startpos_examples-xiangqi" data-toggle="tab">Xiangqi</a>
    </li>
  </ul>

  <div class="tab-content">
    <div class="tab-pane fade active in" id="board-startpos_examples-janggi">
      <pre><code class="json">[
  ["j:r", "j:m", "j:e", "j:s",  null, "j:s", "j:e", "j:m", "j:r"],
  [ null,  null,  null,  null, "j:_g",  null,  null,  null,  null],
  [ null, "j:p",  null,  null,  null,  null,  null, "j:p",  null],
  ["j:j",  null, "j:j",  null, "j:j",  null, "j:j",  null, "j:j"],
  [ null,  null,  null,  null,  null,  null,  null,  null,  null],
  [ null,  null,  null,  null,  null,  null,  null,  null,  null],
  ["J:J",  null, "J:J",  null, "J:J",  null, "J:J",  null, "J:J"],
  [ null, "J:P",  null,  null,  null,  null,  null, "J:P",  null],
  [ null,  null,  null,  null, "J:_G",  null,  null,  null,  null],
  ["J:R", "J:M", "J:E", "J:S",  null, "J:S", "J:E", "J:M", "J:R"]
]</code></pre>
              </div>

              <div class="tab-pane fade" id="board-startpos_examples-makruk">
                <pre><code class="json">[
  ["m:r", "m:n", "m:b", "m:q", "m:_k", "m:b", "m:n", "m:r"],
  [ null,  null,  null,  null,  null,  null,  null,  null],
  ["m:p", "m:p", "m:p", "m:p", "m:p", "m:p", "m:p", "m:p"],
  [ null,  null,  null,  null,  null,  null,  null,  null],
  [ null,  null,  null,  null,  null,  null,  null,  null],
  ["M:P", "M:P", "M:P", "M:P", "M:P", "M:P", "M:P", "M:P"],
  [ null,  null,  null,  null,  null,  null,  null,  null],
  ["M:R", "M:N", "M:B", "M:_K", "M:Q", "M:B", "M:N", "M:R"]
]</code></pre>
              </div>

              <div class="tab-pane fade" id="board-startpos_examples-shogi">
                <pre><code class="json">[
  ["s:l", "s:n", "s:s", "s:g", "s:_k", "s:g", "s:s", "s:n", "s:l"],
  [ null, "s:r",  null,  null,  null,  null,  null, "s:b",  null],
  ["s:p", "s:p", "s:p", "s:p", "s:p", "s:p", "s:p", "s:p", "s:p"],
  [ null,  null,  null,  null,  null,  null,  null,  null,  null],
  [ null,  null,  null,  null,  null,  null,  null,  null,  null],
  [ null,  null,  null,  null,  null,  null,  null,  null,  null],
  ["S:P", "S:P", "S:P", "S:P", "S:P", "S:P", "S:P", "S:P", "S:P"],
  [ null, "S:B",  null,  null,  null,  null,  null, "S:R",  null],
  ["S:L", "S:N", "S:S", "S:G", "S:_K", "S:G", "S:S", "S:N", "S:L"]
]</code></pre>
              </div>

              <div class="tab-pane fade" id="board-startpos_examples-western">
                <pre><code class="json">[
  ["w:r", "w:n", "w:b", "w:q", "w:_k", "w:b", "w:n", "w:r"],
  ["w:p", "w:p", "w:p", "w:p", "w:p", "w:p", "w:p", "w:p"],
  [ null,  null,  null,  null,  null,  null,  null,  null],
  [ null,  null,  null,  null,  null,  null,  null,  null],
  [ null,  null,  null,  null,  null,  null,  null,  null],
  [ null,  null,  null,  null,  null,  null,  null,  null],
  ["W:P", "W:P", "W:P", "W:P", "W:P", "W:P", "W:P", "W:P"],
  ["W:R", "W:N", "W:B", "W:Q", "W:_K", "W:B", "W:N", "W:R"]
]</code></pre>
              </div>

              <div class="tab-pane fade" id="board-startpos_examples-xiangqi">
                <pre><code class="json">[
  ["x:r", "x:h", "x:e", "x:a", "x:_g", "x:a", "x:e", "x:h", "x:r"],
  [ null,  null,  null,  null,  null,  null,  null,  null,  null],
  [ null, "x:c",  null,  null,  null,  null,  null, "x:c",  null],
  ["x:s",  null, "x:s",  null, "x:s",  null, "x:s",  null, "x:s"],
  [ null,  null,  null,  null,  null,  null,  null,  null,  null],
  [ null,  null,  null,  null,  null,  null,  null,  null,  null],
  ["X:S",  null, "X:S",  null, "X:S",  null, "X:S",  null, "X:S"],
  [ null, "X:C",  null,  null,  null,  null,  null, "X:C",  null],
  [ null,  null,  null,  null,  null,  null,  null,  null,  null],
  ["X:R", "X:H", "X:E", "X:A", "X:_G", "X:A", "X:E", "X:H", "X:R"]
]</code></pre>
              </div>
            </div>
</div>

### Moves <small>on the chessboard</small>

The reserved "`moves`" property is REQUIRED.

The moves themselves are given as an ordered set of _actions_ in [portable action notation](Portable-Action-Notation).  A move MUST contain at least one action.

For example, the following move is composed of 2 actions:

    [[ "capture", 42, 43 ], [ "promote", 43, "S:+R" ]]

It can be translated into:

> Capture, using the actor on the coordinate 42, the actor on the coordinate 43; and then promote this capturing actor into <q>Shogi promoted Rook</q>.

<div class="sub-title">Several scenarios</div>

#### Drop <small>a Shogi Rook to square 2</small>

Captured actors can be truly _captured_, such as in Shogi.  Thus, they are retained <q>in hand</q>, and can be brought back into play under the capturing player's control.  On any turn, instead of moving an actor on the board, a player MAY take an actor that had been previously captured and place it, unpromoted side up, on any empty square, facing the opposing side.  The actor is then part of the forces controlled by that player.  This is termed _dropping_ the actor, or just a _drop_.

Given the following position:

    [
      [null, null, null, null, null, null],
      [null, null, null, null, null, null],
      [null, null, null, null, null, null],
      [null, null, null, null, null, null]
    ]

When this move, consisting of one action, is played:

    [[ "drop", "S:R", 2 ]]

Then the position becomes:

    [
      [null, null, "S:R", null, null, null],
      [null, null, null, null, null, null],
      [null, null, null, null, null, null],
      [null, null, null, null, null, null]
    ]

#### Capture <small>an actor</small>

Let's detach an opponent's actor from the board by taking it with a friendly actor:

Given the following position:

    [
      ["s:r", "S:P", null, null, null, null],
      [null, null, null, null, null, null],
      [null, null, null, null, null, null],
      [null, null, null, null, null, null],
      [null, null, null, null, null, null]
    ]

When this move, consisting of one action, is played:

    [[ "capture", 0, 1 ]]

Then the position becomes:

    [
      [null, "s:r", null, null, null, null],
      [null, null, null, null, null, null],
      [null, null, null, null, null, null],
      [null, null, null, null, null, null],
      [null, null, null, null, null, null]
    ]

<span class="label label-info">Captured actors:</span>
The name of captured actors MUST change; e.g., at Shogi, the new side of captured actors can be deduced from the case.

#### Shift <small>an actor on the board</small>

Let's transfer a friendly actor (<q>Xiangqi Chariot, Black</q>) from its square to an _empty square_ of the board.

Given the following position:

    [
      ["x:r", null, null, null, null, null],
      [null, null, null, null, null, null],
      [null, null, null, null, null, null],
      [null, null, null, null, null, null],
      [null, null, null, null, null, null]
    ]

When this move is played:

    [[ "shift", 0, 6 ]]

Then the position becomes:

    [
      [null, null, null, null, null, null],
      ["x:r", null, null, null, null, null],
      [null, null, null, null, null, null],
      [null, null, null, null, null, null],
      [null, null, null, null, null, null]
    ]

#### Promote <small>an actor</small>

Move to promote a Western <q>Pawn</q> to a <q>Queen</q>:

    [[ "shift", 15, 20 ], [ "promote", 20, "w:q" ]]

Move to promote a Shogi <q>Pawn</q>:

    [[ "shift", 1, 2 ], [ "promote", 2, "s:+p" ]]

### Rules of chess <small>governing the play of the game</small>

The reserved "`rule`" property is OPTIONAL.

The "`rule`" property specifies the laws of chess being used.

### Version of the document

The reserved "`version`" property is REQUIRED.

The "`version`" property specifies the version of PCN being used.

For example, here is the [<q>Immortal Game</q>](//en.wikipedia.org/wiki/Immortal_Game) in PCN format:

    {
      "side_top_name":    "Lionel, Kieseritsky",
      "side_bottom_name": "Anderssen, Adolph",

      "state": "checkmate",
      "started_at": "1851-06-21T16:00:00Z",
      "startpos": [
        [ "♜" , "♞" , "♝" , "♛" , "♚" , "♝" , "♞" , "♜" ],
        [ "♟" , "♟" , "♟" , "♟" , "♟" , "♟" , "♟" , "♟" ],
        [ null, null, null, null, null, null, null, null],
        [ null, null, null, null, null, null, null, null],
        [ null, null, null, null, null, null, null, null],
        [ null, null, null, null, null, null, null, null],
        [ "♙" , "♙" , "♙" , "♙" , "♙" , "♙" , "♙" , "♙" ],
        [ "♖" , "♘" , "♗" , "♕" , "♔" , "♗" , "♘" , "♖" ]
      ],
      "moves": [
        [[ "shift", 52, 36 ]],
        [[ "shift", 12, 28 ]],
        [[ "shift", 53, 37 ]],
        [[ "capture", 28, 37 ]],
        [[ "shift", 61, 34 ]],
        [[ "shift", 3, 39 ]],
        [[ "shift", 60, 61 ]],
        [[ "shift", 9, 25 ]],
        [[ "capture", 34, 25 ]],
        [[ "shift", 6, 21 ]],
        [[ "shift", 62, 45 ]],
        [[ "shift", 39, 23 ]],
        [[ "shift", 51, 43 ]],
        [[ "shift", 21, 31 ]],
        [[ "shift", 45, 39 ]],
        [[ "shift", 23, 30 ]],
        [[ "shift", 39, 29 ]],
        [[ "shift", 10, 18 ]],
        [[ "shift", 54, 38 ]],
        [[ "shift", 31, 21 ]],
        [[ "shift", 63, 62 ]],
        [[ "capture", 18, 25 ]],
        [[ "shift", 55, 39 ]],
        [[ "shift", 30, 22 ]],
        [[ "shift", 39, 31 ]],
        [[ "shift", 22, 30 ]],
        [[ "shift", 59, 45 ]],
        [[ "shift", 21, 6 ]],
        [[ "capture", 58, 37 ]],
        [[ "shift", 30, 21 ]],
        [[ "shift", 57, 42 ]],
        [[ "shift", 5, 26 ]],
        [[ "shift", 42, 27 ]],
        [[ "capture", 21, 49 ]],
        [[ "shift", 37, 19 ]],
        [[ "capture", 26, 62 ]],
        [[ "shift", 36, 28 ]],
        [[ "capture", 49, 56 ]],
        [[ "shift", 61, 52 ]],
        [[ "shift", 1, 16 ]],
        [[ "capture", 29, 14 ]],
        [[ "shift", 4, 3 ]],
        [[ "shift", 45, 21 ]],
        [[ "capture", 6, 21 ]],
        [[ "shift", 19, 12 ]]
      ],
      "rule": "20140101",
      "version": "1.0.0"
    }

* * *

<!-- div class="sub-title">See also</div>

* [An implementation in Ruby](//github.com/sashite/pcn.rb) -->

<div class="sub-title">Informative References</div>

This work is influenced by several documents.

* [Portable Action Notation](Portable-Action-Notation)
* [Portable Game Notation](//www.saremba.de/chessgml/standards/pgn/pgn-complete.htm)

<div class="sub-title">Contributing</div>

Want to make this page better?  [Make your changes](//github.com/sashite/open-standards.md/edit/master/docs/Portable-Chess-Notation.md) and submit a hug request.
