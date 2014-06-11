# Forsyth–Edwards Expanded Notation <small>Specification and Implementation Guide</small>

A general purpose <abbr title="American Standard Code for Information Interchange">ASCII</abbr>-based format for defining chess variants' board positions.

<dl class="dl-horizontal">
  <dt>Created</dt>
  <dd><time datetime="2013-03-29T23:17:44Z">29 March 2013</time></dd>

  <dt>Updated</dt>
  <dd><time datetime="2014-05-28T02:41:40Z">28 May 2014</time></dd>

  <dt>Status</dt>
  <dd>beta</dd>

  <dt>Author</dt>
  <dd>Cyril Wack (<a rel="external" href="//twitter.com/cyri_">@cyri_</a>)</dd>
</dl>

<div class="alert alert-warning">
  <button type="button" class="close" data-dismiss="alert">&times;</button>
  <strong>This is a work in progress!</strong>
  This is a beta document and may be updated at any time.
</div>

* * *

<div class="sub-title">Abstract</div>

This document proposes a format for representing most **chessboard positions**, as <q>pictures of game at a given moment</q>.

<div class="sub-title">Status of this document</div>

This document is a beta of the FEEN specification.

<div class="sub-title">Copyright Notice</div>

The content of this page is licensed under the [Creative Commons Attribution 3.0 License](//creativecommons.org/licenses/by/3.0/), and code samples are licensed under the [Apache 2.0 License](//www.apache.org/licenses/LICENSE-2.0).

* * *

## Introduction

<abbr title="Forsyth–Edwards Expanded Notation">FEEN</abbr> (<q>Forsyth–Edwards Expanded Notation</q>) is a lightweight, <abbr title="American Standard Code for Information Interchange">ASCII</abbr>-based format that gives a consistent and easy way to represent most chessboard positions between two-players.

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
  * The system MUST handle chessboards of any dimension.
    Thus, it SHOULD be able to represent board dimensions such as: 2D, 3D, etc.
  * The system MUST handle chessboards of any size.
    Thus, it SHOULD be able to represent board sizes such as: 8x8, 9x9, 9x9x9, etc.
6. Finally, the system SHOULD handle the same kinds and amounts of data that are already handled by existing chess software and by print media.

## How to serve FEEN

The use of the file suffix "`.feen`" is RECOMMENDED for files containing FEEN data.

When serving FEEN over HTTP, the media type "`application/vnd.feen`" is RECOMMENDED.

## <span id="resource">Notation</span>

A FEEN description MUST have five fields:

1. Flatten board
2. Active side
3. Captured actors
4. Castling availability
5. En passant

Each field MUST be composed only of ASCII characters, and non-blank printing.  Adjacent fields MUST be separated by a single ASCII space character (i.e., "` `").  When a field is blank, it MUST be "`-`".

The length of a FEEN position description varies somewhat according to the position and the number of actors.

### Flatten board

Actors MUST be identified by a string; e.g., "<abbr title="Western Pawn, White">`W:P`</abbr>", "<abbr title="Western Knight, White">`W:N`</abbr>", "<abbr title="Xiangqi Soldier, Black">`x:s`</abbr>", "<abbr title="Makruk King, White">`M:_K`</abbr>", "<abbr title="Dai dai Shogi promoted White elephant, Gote">`dai_dai_shogi:+we`</abbr>".

Blank squares are noted using digits 1 through n (where n is the number of blank squares on the last dimension).  A comma character "`,`" is used to separate actors, and between each dimension, solidus characters "`/`" are used.

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

This field MUST contain a single letter: "`b`" means <q>bottom</q> moves next, "`t`" means <q>top</q>.

### Captured actors

Captured actors MUST be listed here.  The actors are listed in alphabetical order, separated by a comma character "`,`".

If neither player has any captured actors, for instance on an initial position, this MUST be "`-`".

<span class="label label-info">Note:</span>
Captured actors are said to be <q>in hand</q> at Shogi.

Example from a game with two Shogi players, in a position where bottom (<ruby lang="ja">先手<rt lang="en">Sente</rt></ruby>) had captured 1 Rook, 1 Gold and 4 Pawns, while top (<ruby lang="ja">後手<rt lang="en">Gote</rt></ruby>) has captured 2 Bishops, 2 Silvers and 3 Pawns:

    S:G,S:P,S:P,S:P,S:P,S:R,s:b,s:b,s:p,s:p,s:p,s:s,s:s

### Castling availability

Every squares of unmoved rooks of an unmoved King MUST be listed (separated by a comma character "`,`"), if this King is able to castling (such as in Western variant).

#### Example

Given the following content:

    0,7,63

Then the "<abbr title="Western King, White">`♔`</abbr>" actor is able to perform the <em>kingside rook</em>, while the "<abbr title="Western King, Black">`♚`</abbr>" actor is able to perform the <em>kingside rook</em> and the <em>queenside rook</em>.

Considering that the game is between two Western players on a 8x8 chessboard, bottom player MAY moved his Queen's "<abbr title="Western Rook, White">`♖`</abbr>" actor.

### En passant

[Unlike in FEN](//www.mychess.de/ChessNotation.htm), <q>en passant</q> field MUST be recorded only when there is a pawn in position to make an en passant capture.  In other words, an en passant target square MUST be given if and only if:

* the last move was a pawn advance of two squares;
* there is at least one pawn of the opposing side that may immediately execute the <q>en passant</q> capture.

The reason being that is to avoid a duplicated FEEN position while both resulting positions have the same gameplay.

## Example

### Scenario

Given the following position between two Western players:

    ♜,♞,♝,♛,♚,♝,♞,♜/♟,♟,♟,♟,♟,♟,♟,♟/8/8/8/8/♙,♙,♙,♙,♙,♙,♙,♙/♖,♘,♗,♕,♔,♗,♘,♖ b - 0,7,56,63 -

When this action (in <abbr title="Portable Action Notation">PAN</abbr> format) is applied:

    [ "shift", 52, 36 ]

Then the position becomes:

    ♜,♞,♝,♛,♚,♝,♞,♜/♟,♟,♟,♟,♟,♟,♟,♟/8/8/4,♙,3/8/♙,♙,♙,♙,1,♙,♙,♙/♖,♘,♗,♕,♔,♗,♘,♖ t - 0,7,56,63 -

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

  <li>
    <a href="#data_fields-board_state-startpos_examples-shogi_vs_xiangqi" data-toggle="tab">Shogi vs Western</a>
  </li>
</ul>

<div class="tab-content">
  <div class="tab-pane fade active in" id="data_fields-board_state-startpos_examples-janggi">
    <pre><code class="feen">j:r,j:m,j:e,j:s,1,j:s,j:e,j:m,j:r/4,j:_g,4/1,j:p,5,j:p,1/j:j,1,j:j,1,j:j,1,j:j,1,j:j/9/9/J:J,1,J:J,1,J:J,1,J:J,1,J:J/1,J:P,5,J:P,1/4,J:_G,4/J:R,J:M,J:E,J:S,1,J:S,J:E,J:M,J:R b - - -</code></pre>
  </div>

  <div class="tab-pane fade" id="data_fields-board_state-startpos_examples-makruk">
    <pre><code class="feen">m:r,m:n,m:b,m:q,m:_k,m:b,m:n,m:r/8/m:p,m:p,m:p,m:p,m:p,m:p,m:p,m:p/8/8/M:P,M:P,M:P,M:P,M:P,M:P,M:P,M:P/8/M:R,M:N,M:B,M:_K,M:Q,M:B,M:N,M:R b - - -</code></pre>
  </div>

  <div class="tab-pane fade" id="data_fields-board_state-startpos_examples-shogi">
    <pre><code class="feen">s:l,s:n,s:s,s:g,s:_k,s:g,s:s,s:n,s:l/1,s:r,5,s:b,1/s:p,s:p,s:p,s:p,s:p,s:p,s:p,s:p,s:p/9/9/9/S:P,S:P,S:P,S:P,S:P,S:P,S:P,S:P,S:P/1,S:B,5,S:R,1/S:L,S:N,S:S,S:G,S:_K,S:G,S:S,S:N,S:L b - - -</code></pre>
  </div>

  <div class="tab-pane fade" id="data_fields-board_state-startpos_examples-western">
    <pre><code class="feen">w:r,w:n,w:b,w:q,w:_k,w:b,w:n,w:r/w:p,w:p,w:p,w:p,w:p,w:p,w:p,w:p/8/8/8/8/W:P,W:P,W:P,W:P,W:P,W:P,W:P,W:P/W:R,W:N,W:B,W:Q,W:_K,W:B,W:N,W:R b - 0,7,56,63 -</code></pre>
  </div>

  <div class="tab-pane fade" id="data_fields-board_state-startpos_examples-xiangqi">
    <pre><code class="feen">x:r,x:h,x:e,x:a,x:_g,x:a,x:e,x:h,x:r/9/1,x:c,5,x:c,1/x:s,1,x:s,1,x:s,1,x:s,1,x:s/9/9/X:S,1,X:S,1,X:S,1,X:S,1,X:S/1,X:C,5,X:C,1/9/X:R,X:H,X:E,X:A,X:_G,X:A,X:E,X:H,X:R b - - -</code></pre>
  </div>

  <div class="tab-pane fade" id="data_fields-board_state-startpos_examples-shogi_vs_xiangqi">
    <pre><code class="feen">w:r,w:n,w:b,w:q,w:_k,w:m,w:b,w:n,w:r/w:p,w:p,w:p,w:p,w:p,w:p,w:p,w:p,w:p/9/9/9/9/S:P,S:P,S:P,S:P,S:P,S:P,S:P,S:P,S:P/1,S:B,5,S:R,1/S:L,S:N,S:S,S:G,S:_K,S:G,S:S,S:N,S:L b - 0,7 -</code></pre>
  </div>
</div>

* * *

<div class="sub-title">Informative References</div>

This work is influenced by several documents.

* [<abbr title="Portable Action Notation">PAN</abbr> format](Portable-Action-Notation)
* [<abbr title="Forsyth–Edwards Notation">FEN</abbr> for Shogi (<abbr title="Shogi FEN">SFEN</abbr>)](//www.glaurungchess.com/shogi/usi.html)
* [<abbr title="Forsyth–Edwards Notation">FEN</abbr> for Western](//en.wikipedia.org/wiki/Forsyth%E2%80%93Edwards_Notation)
* [<abbr title="Forsyth–Edwards Notation">FEN</abbr> for Xiangqi](//www.wxf.org/xq/computer/fen.pdf)

<div class="sub-title">Contributing</div>

Want to make this page better? [Make your changes](//github.com/sashite/open-standards.md/edit/master/docs/Forsyth-Edwards-Expanded-Notation.md) and submit a hug request.
