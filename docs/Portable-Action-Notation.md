# Portable Action Notation <small>Specification and Implementation Guide</small>

A general purpose JSON-based format for storing actions of most abstract strategy board games.

<dl class="dl-horizontal">
  <dt>Created</dt>
  <dd><time datetime="2014-03-08T01:23:45Z">8 March 2014</time></dd>

  <dt>Updated</dt>
  <dd><time datetime="2014-06-28T23:42:34Z">28 June 2014</time></dd>

  <dt>Status</dt>
  <dd>beta</dd>

  <dt>Author</dt>
  <dd><a rel="external" href="//cyril.io">Cyril Wack</a></dd>
</dl>

<div class="alert alert-warning">
  <button type="button" class="close" data-dismiss="alert">&times;</button>
  <strong>This is a work in progress!</strong>
  This is a beta document and may be updated at any time.
</div>

* * *

<div class="sub-title">Abstract</div>

This document proposes a format for representing _actions_ of most abstract strategy **board games**.

<div class="sub-title">Status of this document</div>

This document is a beta of the PAN specification.

<div class="sub-title">Copyright Notice</div>

The content of this page is licensed under the [Creative Commons Attribution 3.0 License](//creativecommons.org/licenses/by/3.0/), and code samples are licensed under the [Apache 2.0 License](//www.apache.org/licenses/LICENSE-2.0).

* * *

## Introduction

<abbr title="Portable Action Notation">PAN</abbr> (<q>Portable Action Notation</q>) is a lightweight, <abbr title="JavaScript Object Notation">JSON</abbr>-based format that gives a consistent and easy way to represent actions of most abstract strategy board games.

Able to describe multidimensional actions, easy for humans to read and write, and easy for machines to import and export, it is completely laws of game independent and compatible with most abstract strategy board games such as [Draughts](//en.wikipedia.org/wiki/Draughts), [Go](//en.wikipedia.org/wiki/Go_(game)) and the main chess variants, including [<ruby lang="ko">장기<rt lang="en">Janggi</rt></ruby>](//en.wikipedia.org/wiki/Janggi), [<ruby lang="th">หมากรุก<rt lang="en">Makruk</rt></ruby>](//en.wikipedia.org/wiki/Makruk), [<ruby lang="ja">将棋<rt lang="en">Shogi</rt></ruby>](//en.wikipedia.org/wiki/Shogi), [Western](//en.wikipedia.org/wiki/Chess), [<ruby lang="zh">象棋<rt lang="en">Xiangqi</rt></ruby>](//en.wikipedia.org/wiki/Xiangqi).  These properties make PAN an ideal data-interchange format for storing actions of most actors from abstract strategy board games such as pawns, pieces, kings, stones.

### Notational conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](//tools.ietf.org/html/rfc2119).

#### Naming the board squares

Squares MUST be identified by a coordinate from an array that is a one-dimensional flattening of the board.  E.g., on the starting position of a traditional Western chessboard, the square of the White Queen is `59` and the square of the White King is `60`.

## Specification goals

A specification for a portable move notation MUST observe the following criteria:

1. The details of the system MUST be publicly available and free of unnecessary complexity.
2. The details of the system MUST be non-proprietary so that users and software developers are unrestricted by concerns about infringing on intellectual property rights.
3. The system MUST work for a variety of programs.  The format SHOULD be such that it can be used by abstract strategy board game database programs, abstract strategy board game publishing programs, abstract strategy board game server programs, and abstract strategy board game playing programs without being unnecessarily specific to any particular application class.
4. The system MUST handle abstract strategy board games which are playable by multiple sides.
5. The system MUST be easily expandable and scalable.  Examples: new actors, new games, new actions, new rules, etc.
  * The system MUST be cross-variants.
    It SHOULD be able to represent actions where sides MAY play
    different abstract strategy board games such as Janggi, Go, Draughts, Shogi, Western, Xiangqi.
  * The system MUST handle move of boards of any dimension; e.g., 2D, 3D.
  * The system MUST handle move of boards of any size; e.g., 8x8, 9x9, 9x9x9.
  * The system MUST be able to fully describe actor actions according to
    a concerned Laws of Chess and the given chessboard structure.
6. Finally, the system SHOULD handle the same kinds and amounts of data that are already handled by existing chess software and by print media.

## How to serve PAN

The use of the file suffix "`.pan`" is RECOMMENDED for files containing PAN data.

When serving PAN over HTTP, the media type "`application/vnd.pan+json`" is RECOMMENDED.

## <span id="resource">Notation for actions</span>

Several verbs indicate the keyword that in syntax conveys an action.

### Movement

<div class="sub-title">Resource representation</div>

The JSON structure below shows the format of the resource:

    [ {verb}, {src_square}, {dst_square} ]

<div class="sub-title">Properties</div>

The following table defines the properties that appear in this resource:

| Property name  | Value    | Description |
| -------------- | -------- | ----------- |
| "`verb`"       | a string | Indicates the keyword that in syntax conveys an action.  The possible values are "`shift`", "`capture`", and "`remove`". |
| "`src_square`" | unsigned integer | Indicates a source square occupied by an actor. |
| "`dst_square`" | unsigned integer | Indicates a target square. |

#### Shift

Act of transfering an actor from a source square to a destination square of the board.

<div class="alert alert-warning">
  <strong>Integrity constraints with the board:</strong>

  <ul>
    <li>The given source square MUST be occupied by an actor.</li>
    <li>The given destination square MUST be free.</li>
  </ul>
</div>

#### Remove

Act of cleaning a destination square of the board, and then transfering an actor from a source square to this destination square.

<div class="alert alert-warning">
  <strong>Integrity constraints with the board:</strong>

  <ul>
    <li>The given source square MUST be occupied by an actor.</li>
    <li>The given destination square MUST be occupied by an actor.</li>
  </ul>
</div>

#### Capture

Act of moving the actor from a destination square of the board to the actors in hand, and then transfering an actor from a source square to this destination square.

<span class="label label-info">Captured actors:</span>
Captured actors can be truly _captured_, such as in Shogi.  Thus, they are retained <q>in hand</q>.

<div class="alert alert-warning">
  <strong>Integrity constraints with the board:</strong>

  <ul>
    <li>The given source square MUST be occupied by an actor.</li>
    <li>The given destination square MUST be occupied by an actor.</li>
  </ul>
</div>

### Promotion

Act of promoting an actor.

<div class="alert alert-warning">
  <strong>Integrity constraints with the board:</strong>

  <p>
    The given source square MUST be occupied by an actor.
  </p>
</div>

<div class="sub-title">Resource representation</div>

The JSON structure below shows the format of the resource:

    [ "promote", {src_square}, {actor} ]

<div class="sub-title">Properties</div>

The following table defines the properties that appear in this resource:

| Property name  | Value            | Description                                            |
| -------------- | ---------------- | ------------------------------------------------------ |
| "`src_square`" | unsigned integer | Indicates a source square occupied by an actor.        |
| "`actor`"      | a string         | Indicates the new actor to replace on the same square. |

### Drops

Act of selecting an actor in hand and place it on a square.

<div class="sub-title">Resource representation</div>

The JSON structure below shows the format of the resource:

    [ "drop", {actor}, {dst_square} ]

<div class="sub-title">Properties</div>

The following table defines the properties that appear in this resource:

| Property name  | Value            | Description                     |
| -------------- | ---------------- | ------------------------------- |
| "`actor`"      | a string         | Indicates an actor in hand.     |
| "`dst_square`" | unsigned integer | Indicates a free target square. |

## Example

List of actions of the [<q>Immortal Game</q>](//en.wikipedia.org/wiki/Immortal_Game), in PAN format:

1. `[ "shift", 52, 36 ]`
2. `[ "shift", 12, 28 ]`
3. `[ "shift", 53, 37 ]`
4. `[ "remove", 28, 37 ]`
5. `[ "shift", 61, 34 ]`
6. `[ "shift", 3, 39 ]`
7. `[ "shift", 60, 61 ]`
8. `[ "shift", 9, 25 ]`
9. `[ "remove", 34, 25 ]`
10. `[ "shift", 6, 21 ]`
11. `[ "shift", 62, 45 ]`
12. `[ "shift", 39, 23 ]`
13. `[ "shift", 51, 43 ]`
14. `[ "shift", 21, 31 ]`
15. `[ "shift", 45, 39 ]`
16. `[ "shift", 23, 30 ]`
17. `[ "shift", 39, 29 ]`
18. `[ "shift", 10, 18 ]`
19. `[ "shift", 54, 38 ]`
20. `[ "shift", 31, 21 ]`
21. `[ "shift", 63, 62 ]`
22. `[ "remove", 18, 25 ]`
23. `[ "shift", 55, 39 ]`
24. `[ "shift", 30, 22 ]`
25. `[ "shift", 39, 31 ]`
26. `[ "shift", 22, 30 ]`
27. `[ "shift", 59, 45 ]`
28. `[ "shift", 21, 6 ]`
29. `[ "remove", 58, 37 ]`
30. `[ "shift", 30, 21 ]`
31. `[ "shift", 57, 42 ]`
32. `[ "shift", 5, 26 ]`
33. `[ "shift", 42, 27 ]`
34. `[ "remove", 21, 49 ]`
35. `[ "shift", 37, 19 ]`
36. `[ "remove", 26, 62 ]`
37. `[ "shift", 36, 28 ]`
38. `[ "remove", 49, 56 ]`
39. `[ "shift", 61, 52 ]`
40. `[ "shift", 1, 16 ]`
41. `[ "remove", 29, 14 ]`
42. `[ "shift", 4, 3 ]`
43. `[ "shift", 45, 21 ]`
44. `[ "remove", 6, 21 ]`
45. `[ "shift", 19, 12 ]`

* * *

<div class="sub-title">See also</div>

* [An implementation in Ruby](//github.com/sashite/pan.rb)

<div class="sub-title">Informative References</div>

This work is influenced by several documents.

* [Algebraic Notation](//en.wikipedia.org/wiki/Algebraic_notation_(chess))
* [Smart Game Format](//www.red-bean.com/sgf/)

<div class="sub-title">Contributing</div>

Want to make this page better?  [Make your changes](//github.com/sashite/open-standards.md/edit/master/docs/Portable-Action-Notation.md) and submit a hug request.
