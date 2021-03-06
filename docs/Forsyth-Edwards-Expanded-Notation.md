# Forsyth–Edwards Expanded Notation

A general purpose <abbr title="Universal Character Set Transformation Format - 8 bits">UTF-8</abbr>-based format for defining chess variants' board positions.

<dl class="dl-horizontal">
  <dt>Created</dt>
  <dd><time datetime="2013-03-29T23:17:44Z">March 29, 2013</time></dd>

  <dt>Updated</dt>
  <dd><time datetime="2020-05-11T17:29:55Z">May 11, 2020</time></dd>

  <dt>Status</dt>
  <dd>beta</dd>
</dl>

<div class="alert alert-warning">
  <strong>This is a work in progress!</strong>
  This is a beta document and may be updated at any time.
</div>

***

<div class="sub-title">Abstract</div>

This document proposes a format for representing most **chessboard positions**, as <q>pictures of game at a given moment</q>.

<div class="sub-title">Status of this document</div>

This document is a beta of the FEEN specification.

<div class="sub-title">Copyright notice</div>

Except as otherwise noted, the content of this page is licensed under the [Creative Commons Attribution 4.0 License](https://creativecommons.org/licenses/by/4.0/), and code samples are licensed under the [Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0).

***

## Introduction

<abbr title="Forsyth–Edwards Expanded Notation">FEEN</abbr> (<q>Forsyth–Edwards Expanded Notation</q>) is a lightweight, <abbr title="Universal Character Set Transformation Format - 8 bits">UTF-8</abbr>-based format that gives a consistent and easy way to represent most chessboard positions between two-players.

Inspired by <abbr title="Forsyth–Edwards Notation">FEN</abbr> (<q>Forsyth–Edwards Notation</q>), FEEN is able to describe both multidimensional positions and related state, easy for humans to read and write, and easy for machines to import and export, it is completely laws of chess independent and compatible with the main chess variants, including [<ruby lang="ko">장기<rt lang="en">Janggi</rt></ruby>](//en.wikipedia.org/wiki/Janggi), [<ruby lang="th">หมากรุก<rt lang="en">Makruk</rt></ruby>](//en.wikipedia.org/wiki/Makruk), [<ruby lang="ja">将棋<rt lang="en">Shogi</rt></ruby>](//en.wikipedia.org/wiki/Shogi), [Western](//en.wikipedia.org/wiki/Chess), [<ruby lang="zh">象棋<rt lang="en">Xiangqi</rt></ruby>](//en.wikipedia.org/wiki/Xiangqi), and many others.  These properties make FEEN an ideal data-interchange format for recording chessboard positions.

### Notational Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](//tools.ietf.org/html/rfc2119).

## Specification goals

A specification for a Forsyth–Edwards expanded notation MUST observe the following criteria:

1. The details of the system MUST be publicly available and free of unnecessary complexity.
2. The details of the system MUST be non-proprietary so that users and software developers are unrestricted by concerns about infringing on intellectual property rights.
3. The system MUST work for a variety of programs.  The format SHOULD be such that it can be used by chess database programs, chess publishing programs, chess server programs, and chessplaying programs without being unnecessarily specific to any particular application class.
4. The system MUST handle positions of games which are playable by many sides.
5. The system MUST be easily expandable and scalable.  Examples: new chessboards, new opening positions, new actors, new moves, new rules, etc.
  * The system MUST be cross-variants.
    It SHOULD be able to represent boards where sides MAY play
    different chess variants such as Janggi, Makruk, Shogi, Western, Xiangqi.
  * The system MUST handle chessboards of any dimension, such as: 2D, 3D, etc.
  * The system MUST handle chessboards of any size, such as: 8x8, 9x9, 9x9x9, etc.
6. Finally, the system SHOULD handle the same kinds and amounts of data that are already handled by existing chess software and by print media.

## How to serve FEEN

The use of the file suffix "`.feen`" is RECOMMENDED for files containing FEEN data.

When serving FEEN over HTTP, the media type "`application/vnd.feen`" is RECOMMENDED.

## <span id="resource">Notation for board positions</span>

A FEEN description MUST have *three fields*:

1. Flatten board
2. Active side
3. Captured actors

Each field MUST be composed only of UTF-8 characters, and non-blank printing.  Adjacent fields MUST be separated by a single UTF-8 space character (i.e., " ").  When a field is blank, it MUST be "`-`".

The length of a FEEN position description varies somewhat according to the position and the number of actors.

### Flatten board

Actors MUST be identified by a single character; e.g., "<abbr title="Western Pawn, White">`P`</abbr>", "<abbr title="Western Knight, White">`N`</abbr>", "<abbr title="Xiangqi Soldier, Black">`卒`</abbr>", "<abbr title="Makruk King, White">`♔`</abbr>", "<abbr title="Dai dai Shogi Fragrant elephant, Sente">`象`</abbr>".

Blank squares are noted using digits 1 through n (where n is the number of blank squares on the last dimension).  An empty string "" is used to separate actors, and between each dimension, solidus characters "`/`" are used.

#### Empty chessboard examples

<ul class="nav nav-tabs">
  <li class="active">
    <a href="#data_fields-board_state-empty_chessboard_examples-6_size" data-toggle="tab">6 size</a>
  </li>

  <li>
    <a href="#data_fields-board_state-empty_chessboard_examples-4x8_size" data-toggle="tab">4x8 size</a>
  </li>

  <li>
    <a href="#data_fields-board_state-empty_chessboard_examples-7x7_size" data-toggle="tab">7x7 size</a>
  </li>

  <li>
    <a href="#data_fields-board_state-empty_chessboard_examples-5x5x5_size" data-toggle="tab">5x5x5 size</a>
  </li>
</ul>

<div class="tab-content">
  <div class="tab-pane fade active in" id="data_fields-board_state-empty_chessboard_examples-6_size">
    <pre><code class="feen">6</code></pre>
  </div>

  <div class="tab-pane fade" id="data_fields-board_state-empty_chessboard_examples-4x8_size">
    <pre><code class="feen">8/8/8/8</code></pre>
  </div>

  <div class="tab-pane fade" id="data_fields-board_state-empty_chessboard_examples-7x7_size">
    <pre><code class="feen">7/7/7/7/7/7/7</code></pre>
  </div>

  <div class="tab-pane fade" id="data_fields-board_state-empty_chessboard_examples-5x5x5_size">
    <pre><code class="feen">5/5/5/5/5//5/5/5/5/5//5/5/5/5/5//5/5/5/5/5//5/5/5/5/5</code></pre>
  </div>
</div>

### Active side

This field MUST contain a single letter: "`B`" means <q>bottom</q> moves next, "`t`" means <q>top</q>.

### Captured actors

Captured actors MUST be listed here.  The actors are listed in alphabetical order, separated by an empty string "".

If neither player has any captured actors, for instance on an initial position, this MUST be "`-`".

<span class="label label-info">Note:</span>
Captured actors are said to be <q>in hand</q> at Shogi.

Example from a game with two Shogi players, in a position where bottom (<ruby lang="ja">先手<rt lang="en">Sente</rt></ruby>) had captured 1 Rook, 1 Gold and 4 Pawns, while top (<ruby lang="ja">後手<rt lang="en">Gote</rt></ruby>) has captured 2 Bishops, 2 Silvers and 3 Pawns:

    GPPPPRbbpppss

## Example

### Starting positions

<ul class="nav nav-tabs">
  <li class="active">
    <a href="#data_fields-board_state-startpos_examples-janggi" data-toggle="tab">Janggi</a>
  </li>

  <li>
    <a href="#data_fields-board_state-startpos_examples-makruk" data-toggle="tab">Makruk</a>
  </li>

  <li>
    <a href="#data_fields-board_state-startpos_examples-shogi" data-toggle="tab">Shogi</a>
  </li>

  <li>
    <a href="#data_fields-board_state-startpos_examples-western" data-toggle="tab">Western</a>
  </li>

  <li>
    <a href="#data_fields-board_state-startpos_examples-xiangqi" data-toggle="tab">Xiangqi</a>
  </li>
</ul>

<div class="tab-content">
  <div class="tab-pane fade active in" id="data_fields-board_state-startpos_examples-janggi">
    <pre><code class="feen">rmes1semr/4g4/1p5p1/j1j1j1j1j/9/9/J1J1J1J1J/1P5P1/4G4/RMES1SEMR B -</code></pre>
  </div>

  <div class="tab-pane fade" id="data_fields-board_state-startpos_examples-makruk">
    <pre><code class="feen">rnbqkbnr/8/pppppppp/8/8/PPPPPPPP/8/RNBKQBNR B -</code></pre>
  </div>

  <div class="tab-pane fade" id="data_fields-board_state-startpos_examples-shogi">
    <pre><code class="feen">lnsgkgsnl/1r5b1/ppppppppp/9/9/9/PPPPPPPPP/1B5R1/LNSGKGSNL B -</code></pre>
  </div>

  <div class="tab-pane fade" id="data_fields-board_state-startpos_examples-western">
    <pre><code class="feen">rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR B -</code></pre>
  </div>

  <div class="tab-pane fade" id="data_fields-board_state-startpos_examples-xiangqi">
    <pre><code class="feen">rheagaehr/9/1c5c1/s1s1s1s1s/9/9/S1S1S1S1S/1C5C1/9/RHEAGAEHR B -</code></pre>
  </div>
</div>

### Scenario

Given the following position between two Western players:

    ♜♞♝♛♚♝♞♜/♟♟♟♟♟♟♟♟/8/8/8/8/♙♙♙♙♙♙♙♙/♖♘♗♕♔♗♘♖ B -

When this action (in [<abbr title="Portable Board Diff Notation">PBDN</abbr> format](Portable-Board-Diff-Notation)) is applied:

    [ 52, 36, "♙" ]

Then the position becomes:

    ♜♞♝♛♚♝♞♜/♟♟♟♟♟♟♟♟/8/8/4♙3/8/♙♙♙♙1♙♙♙/♖♘♗♕♔♗♘♖ t -

***

<!-- div class="sub-title">See also</div>

* [An implementation in Ruby](//github.com/sashite/feen.rb) -->

<div class="sub-title">Informative References</div>

This work is influenced by several documents.

* [FEN for Western](//en.wikipedia.org/wiki/Forsyth%E2%80%93Edwards_Notation)
* [FEN for Xiangqi](//www.wxf.org/xq/computer/fen.pdf)
* [The Universal Shogi Interface](//www.glaurungchess.com/shogi/usi.html)

<div class="sub-title">Contributing</div>

Want to make this page better?  [Make your changes](//github.com/sashite/specifications.md/edit/master/docs/Forsyth-Edwards-Expanded-Notation.md) and submit a hug request.
