# Portable Board Diff Notation <small>Specification and Implementation Guide</small>

A general purpose JSON-based format for storing changes between actions of most abstract strategy board games.

<dl class="dl-horizontal">
  <dt>Created</dt>
  <dd><time datetime="2015-07-15T01:23:45Z">July 15, 2015</time></dd>

  <dt>Updated</dt>
  <dd><time datetime="2015-11-10T23:42:34Z">Nov 10, 2015</time></dd>

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

***

<div class="sub-title">Abstract</div>

This document proposes a format for representing _changes between actions_ of most abstract strategy **board games**.

<div class="sub-title">Status of this document</div>

This document is a beta of the PBDN specification.

<div class="sub-title">Copyright Notice</div>

The content of this page is licensed under the [Creative Commons Attribution 3.0 License](//creativecommons.org/licenses/by/3.0/), and code samples are licensed under the [Apache 2.0 License](//www.apache.org/licenses/LICENSE-2.0).

***

## Introduction

<abbr title="Portable Board Diff Notation">PBDN</abbr> (<q>Portable Board Diff Notation</q>) is a lightweight, <abbr title="JavaScript Object Notation">JSON</abbr>-based format that gives a consistent and easy way to represent changes between actions of most abstract strategy board games.

Compatible with multidimensional boards, easy for humans to read and write, and easy for machines to import and export, it is completely laws of game independent and compatible with most abstract strategy board games such as [Draughts](//en.wikipedia.org/wiki/Draughts), [Go](//en.wikipedia.org/wiki/Go_(game)) and the main chess variants, including [<ruby lang="ko">장기<rt lang="en">Janggi</rt></ruby>](//en.wikipedia.org/wiki/Janggi), [<ruby lang="th">หมากรุก<rt lang="en">Makruk</rt></ruby>](//en.wikipedia.org/wiki/Makruk), [<ruby lang="ja">将棋<rt lang="en">Shogi</rt></ruby>](//en.wikipedia.org/wiki/Shogi), [Western](//en.wikipedia.org/wiki/Chess), [<ruby lang="zh">象棋<rt lang="en">Xiangqi</rt></ruby>](//en.wikipedia.org/wiki/Xiangqi).  These properties make PBDN an ideal data-interchange format for storing changes between actions of most actors from abstract strategy board games such as pawns, pieces, kings, stones.

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
    It SHOULD be able to represent changes between actions where sides MAY play
    different abstract strategy board games such as Janggi, Go, Draughts, Shogi, Western, Xiangqi.
  * The system MUST handle move of boards of any dimension; e.g., 2D, 3D.
  * The system MUST handle move of boards of any size; e.g., 8x8, 9x9, 9x9x9.
  * The system MUST be able to fully describe actor actions according to
    a concerned Laws of Chess and the given chessboard structure.
6. Finally, the system SHOULD handle the same kinds and amounts of data that are already handled by existing chess software and by print media.

## How to serve PBDN

The use of the file suffix "`.pbdn`" is RECOMMENDED for files containing PBDN data.

When serving PBDN over HTTP, the media type "`application/vnd.pbdn+json`" is RECOMMENDED.

## <span id="resource">Notation</span>

<div class="sub-title">Resource representation</div>

The JSON structure below shows the format of the resource:

    [ {src_square}, {dst_square}, {piece_name} ]

<div class="sub-title">Properties</div>

The following table defines the properties that appear in this resource:

| Property name  | Value    | Description |
| -------------- | -------- | ----------- |
| "`src_square`" | unsigned integer | Indicates a source square occupied by an actor. |
| "`dst_square`" | unsigned integer | Indicates a target square. |
| "`piece_name`" | a string | Indicates the name of the actor of the action. |

## Example

List of changes of the [<q>Immortal Game</q>](//en.wikipedia.org/wiki/Immortal_Game), in PBDN format:

1. `[ 52, 36, "W:P" ]`
2. `[ 12, 28, "w:p" ]`
3. `[ 53, 37, "W:P" ]`
4. `[ 28, 37, "w:p" ]`
5. `[ 61, 34, "W:B" ]`
6. `[ 3, 39, "w:q" ]`
7. `[ 60, 61, "W:^K" ]`
8. `[ 9, 25, "w:p" ]`
9. `[ 34, 25, "W:B" ]`
10. `[ 6, 21, "w:n" ]`
11. `[ 62, 45, "W:N" ]`
12. `[ 39, 23, "w:q" ]`
13. `[ 51, 43, "W:P" ]`
14. `[ 21, 31, "w:n" ]`
15. `[ 45, 39, "W:N" ]`
16. `[ 23, 30, "w:q" ]`
17. `[ 39, 29, "W:N" ]`
18. `[ 10, 18, "w:p" ]`
19. `[ 54, 38, "W:P" ]`
20. `[ 31, 21, "w:n" ]`
21. `[ 63, 62, "W:R" ]`
22. `[ 18, 25, "w:p" ]`
23. `[ 55, 39, "W:P" ]`
24. `[ 30, 22, "w:q" ]`
25. `[ 39, 31, "W:P" ]`
26. `[ 22, 30, "w:q" ]`
27. `[ 59, 45, "W:Q" ]`
28. `[ 21, 6, "w:n" ]`
29. `[ 58, 37, "W:B" ]`
30. `[ 30, 21, "w:q" ]`
31. `[ 57, 42, "W:N" ]`
32. `[ 5, 26, "w:b" ]`
33. `[ 42, 27, "W:N" ]`
34. `[ 21, 49, "w:q" ]`
35. `[ 37, 19, "W:B" ]`
36. `[ 26, 62, "w:b" ]`
37. `[ 36, 28, "W:P" ]`
38. `[ 49, 56, "w:q" ]`
39. `[ 61, 52, "W:^K" ]`
40. `[ 1, 16, "w:n" ]`
41. `[ 29, 14, "W:N" ]`
42. `[ 4, 3, "w:^k" ]`
43. `[ 45, 21, "W:Q" ]`
44. `[ 6, 21, "w:n" ]`
45. `[ 19, 12, "W:B" ]`

***

<div class="sub-title">See also</div>

* [An implementation in Ruby](//github.com/sashite/pbdn.rb)

<div class="sub-title">Informative References</div>

This work is influenced by several documents.

* [Portable Action Notation](Portable-Action-Notation)
* [Algebraic Notation](//en.wikipedia.org/wiki/Algebraic_notation_(chess))
* [Smart Game Format](//www.red-bean.com/sgf/)

<div class="sub-title">Contributing</div>

Want to make this page better?  [Make your changes](//github.com/sashite/open-standards.md/edit/master/docs/Portable-Board-Diff-Notation.md) and submit a hug request.
