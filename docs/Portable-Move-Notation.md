# Portable Move Notation

A general purpose JSON-based format for recording moves.

<dl class="dl-horizontal">
  <dt>Created</dt>
  <dd><time datetime="2020-06-14T17:29:55Z">June 14, 2020</time></dd>

  <dt>Updated</dt>
  <dd><time datetime="2020-06-14T17:29:55Z">June 14, 2020</time></dd>

  <dt>Status</dt>
  <dd>beta</dd>
</dl>

<aside class="alert alert-warning">
  <strong>This is a work in progress!</strong>
  This is a beta document and may be updated at any time.
</aside>

***

<div class="sub-title">Abstract</div>

This document proposes a format for representing most **chess** moves.

<div class="sub-title">Status of this document</div>

This document is a beta of the PMN specification.

<div class="sub-title">Copyright notice</div>

Except as otherwise noted, the content of this page is licensed under the [Creative Commons Attribution 4.0 License](https://creativecommons.org/licenses/by/4.0/), and code samples are licensed under the [Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0).

***

## Introduction

<abbr title="Portable Move Notation">PMN</abbr> (<q>Portable Move Notation</q>) is a lightweight, <abbr title="JavaScript Object Notation">JSON</abbr>-based format that gives a consistent and easy way to represent moves in the context of chess games.

Compatible with multidimensional boards, easy for humans to read and write, and easy for machines to import and export, it is completely laws of game independent and compatible with most abstract strategy board games such as [Draughts](//en.wikipedia.org/wiki/Draughts), [Go](//en.wikipedia.org/wiki/Go_(game)) and the main chess variants, including [<ruby lang="ko">장기<rt lang="en">Janggi</rt></ruby>](//en.wikipedia.org/wiki/Janggi), [<ruby lang="th">หมากรุก<rt lang="en">Makruk</rt></ruby>](//en.wikipedia.org/wiki/Makruk), [<ruby lang="ja">将棋<rt lang="en">Shogi</rt></ruby>](//en.wikipedia.org/wiki/Shogi), [Western](//en.wikipedia.org/wiki/Chess), [<ruby lang="zh">象棋<rt lang="en">Xiangqi</rt></ruby>](//en.wikipedia.org/wiki/Xiangqi).  These properties make PMN an ideal data-interchange format for storing changes between actions of most actors from abstract strategy board games such as pawns, pieces, kings, stones.

### Notational conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](//tools.ietf.org/html/rfc2119).

#### Naming the board squares

Squares MUST be identified by a coordinate from an array that is a one-dimensional flattening of the board.  E.g., on the starting position of a traditional Western chessboard, the square of the White Queen is `59` and the square of the White King is `60`.

## How to serve PMN

The use of the file suffix "`.pmn`" is RECOMMENDED for files containing PMN data.

When serving PMN over HTTP, the media type "`application/vnd.pmn+json`" is RECOMMENDED.

## <span id="resource">Move</span>

<div class="sub-title">Resource representation</div>

The JSON structure below shows the format of the resource:

    [ n * <action items> ]

where `n` MUST be greater than or equal to `1`.

## <span id="resource">Action items</span>

<div class="sub-title">Resource representation</div>

The JSON structure below shows the format of the resource:

    {src_square}, {dst_square}, {piece_name}, {piece_hand}

<div class="sub-title">Items</div>

The following table defines the items that appear in this resource:

| Item           | Value               | Required | Default | Description |
| -------------- | ------------------- | -------- | ------- | ----------- |
| [`src_square`](#item-src_square) | an unsigned integer | false    | `null`  | A source square. |
| [`dst_square`](#item-dst_square) | an unsigned integer | true     |         | A target square. |
| [`piece_name`](#item-piece_name) | a string            | true     |         | The name of the piece. |
| [`piece_hand`](#item-piece_hand) | a string            | false    | `null`  | The name of the captured piece in hand. |


<h3 id="item-src_square">Source square</h3>

The "`src_square`" item is OPTIONAL.

The "`src_square`" item represents the coordinate of the source square of the action.

If the value is `null`, the action SHOULD be a [_drop_](#example-drop).

<h3 id="item-dst_square">Target square</h3>

The "`dst_square`" item is REQUIRED.

The "`dst_square`" item represents the coordinate of the target square of the action.


<h3 id="item-piece_name">Piece name</h3>

The "`piece_name`" item is REQUIRED.

The "`piece_name`" item represents the name of the piece of the action.


<h3 id="item-piece_hand">Piece hand</h3>

The "`piece_hand`" item is OPTIONAL.

The "`piece_hand`" item represents the name of the piece that is retained [_in hand_](#example-capture-piece_in_hand) by the player who performed the action.


***


## Examples

<h3 id="example-drop">Drop</h3>

Captured pieces can be truly _captured_, such as in Shogi.  Thus, they are retained <q>in hand</q>, and can be brought back into play under the capturing player's control.  On any turn, instead of moving a piece on the board, a player MAY take a piece that had been previously captured and place it.  The piece is then part of the forces controlled by that player.  This is termed _dropping_ the piece, or just a _drop_.

Given the following position:

    [
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null
    ]

When this move is played:

    [ null, 2, "R", null ]

Then the position becomes:

    [
      null, null, "R", null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null
    ]

<h3 id="example-capture">Capture</h3>

<h4 id="example-capture-en_passant">En passant</h4>

In Western chess, a special pawn capture can occur immediately after a pawn makes a move of two squares from its starting square, and it could have been captured by an enemy pawn had it advanced only one square.

Given the following position:

    [
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, "p", null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      "P", null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null
    ]

After this move from bottom-side player:

    [ 48, 32, "P", null ]

Given this new position:

    [
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      "P", "p", null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null
    ]

Top-side player can capture en passant with the move:

    [ 33, 32, "p", null, 32, 40, "p", null ]

Then the position becomes:

    [
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      "p", null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null
    ]

<h4 id="example-capture-piece_in_hand">Piece in hand</h4>

Given the following position:

    [
      "r", "P", null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null
    ]

When this move is played:

    [ 0, 1, "r", "p", null ]

Then the position becomes:

    [
      null, "r", null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null
    ]

The 4th parameter indicates that the captured "`P`" piece becomes "`p`" and is retained in hand by the player who played.

<h3 id="example-capture-shift">Shift</h3>

Let's transfer a friendly piece (<q>Xiangqi Chariot, Black</q>) from its square to an _empty square_ of the board.

Given the following position:

    [
      "r", null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null
    ]

When this move is played:

    [ 0, 8, "r", null ]

Then the position becomes:

    [
      null, null, null, null, null, null, null, null, "r",
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null
    ]

<h3 id="example-capture-promotion">Promotion</h3>

<h4 id="example-capture-promotion-western_pawn_to_a_queen">Western <q>Pawn</q> to a <q>Queen</q></h4>

Given the following position:

    [
      null, null, null, null, null, null, null, null,
      "P", null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null
    ]

When this move is played:

    [ 6, 0, "Q", null ]

Then the position becomes:

    [
      "Q", null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null
    ]

<h4 id="example-capture-promotion-shogi_pawn_to_a_promoted_shogi_pawn">Shogi <q>Pawn</q> to a promoted Shogi <q>Pawn</q></h4>

Given the following position:

    [
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      "P", null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null
    ]

When this move is played:

    [ 27, 18, "+P", null ]

Then the position becomes:

    [
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      "+P", null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null,
      null, null, null, null, null, null, null, null, null
    ]

***

<div class="sub-title">See also</div>

* [Portable Chess Notation](https://developer.sashite.com/specs/portable-chess-notation)

<div class="sub-title">Implementation</div>

* [Ruby interface for data serialization in PMN format](https://github.com/sashite/portable_move_notation.rb)

<div class="sub-title">Contributing</div>

Want to make this page better?  [Make your changes](https://github.com/sashite/specifications.md/edit/master/docs/Portable-Move-Notation.md) and submit a hug request.
