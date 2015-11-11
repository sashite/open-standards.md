# Chess Position Notation <small>Specification and Implementation Guide</small>

A general purpose <abbr title="American Standard Code for Information Interchange">ASCII</abbr>-based format for defining chess variants' board positions.

<dl class="dl-horizontal">
  <dt>Created</dt>
  <dd><time datetime="2013-06-14T23:17:44Z">June 14, 2014</time></dd>

  <dt>Updated</dt>
  <dd><time datetime="2015-11-11T23:42:34Z">Nov. 11, 2015</time></dd>

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

This document proposes a format for representing a unique _board position_, preventing from duplicates, as defined through the **rules of the game**.

<div class="sub-title">Status of this document</div>

This document is a beta of the CPN specification.

<div class="sub-title">Copyright Notice</div>

The content of this page is licensed under the [Creative Commons Attribution 3.0 License](//creativecommons.org/licenses/by/3.0/), and code samples are licensed under the [Apache 2.0 License](//www.apache.org/licenses/LICENSE-2.0).

***

## Introduction

<abbr title="Chess Position Notation">CPN</abbr> (<q>Chess Position Notation</q>) is plaintext-based format that gives a consistent and easy way to represent most chessboard positions between two-players.

CPN is able to describe both multidimensional positions and related state, easy for machines to import and export, it is laws of chess dependent and compatible with the main chess variants, including [<ruby lang="ko">장기<rt lang="en">Janggi</rt></ruby>](//en.wikipedia.org/wiki/Janggi), [<ruby lang="th">หมากรุก<rt lang="en">Makruk</rt></ruby>](//en.wikipedia.org/wiki/Makruk), [<ruby lang="ja">将棋<rt lang="en">Shogi</rt></ruby>](//en.wikipedia.org/wiki/Shogi), [Western](//en.wikipedia.org/wiki/Chess), [<ruby lang="zh">象棋<rt lang="en">Xiangqi</rt></ruby>](//en.wikipedia.org/wiki/Xiangqi), and many others.  These properties make CPN an ideal data-interchange format for recording canonical chessboard positions.

### Notational Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](//tools.ietf.org/html/rfc2119).

## Specification goals

A specification for a canonical position notation MUST observe the following criteria:

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

## How to serve CPN

The use of the file suffix "`.cpn`" is RECOMMENDED for files containing CPN data.

When serving CPN over HTTP, the media type "`application/vnd.cpn+feen`" is RECOMMENDED.

## <span id="resource">Notation for canonical board positions</span>

A CPN description MUST have the same four fields, joined by "`--`".

### Active side

This field MUST contain a single letter: "`B`" means <q>bottom</q> moves next, "`t`" means <q>top</q>.

### Shape

This field MUST a number for each shape, separated by a comma: "`,`". Example for a 8x8 board: "`8,8`".

### Flatten board

Actors MUST be identified in to the [GAN format](General-Actor-Notation).

A comma ("`,`") is used to separate them.

### Pieces in hand

The pieces are sorted in the alphabetical order and separated by a comma ("`,`").

## Canonical position hash coding

The position of boards can be named by a unique identifier, the <abbr title="Chess Position Hash">CPH</abbr> (<q>Chess Position Hash</q>).

This unique identifier is computed from the SHA1 of a CPN string.

***

<div class="sub-title">See also</div>

* [An implementation in Ruby](//github.com/sashite/cpn.rb)

<div class="sub-title">Informative References</div>

This work is influenced by several documents.

* [<abbr title="General-Actor-Notation">GAN</abbr> format](General-Actor-Notation)

<div class="sub-title">Contributing</div>

Want to make this page better?  [Make your changes](//github.com/sashite/open-standards.md/edit/master/docs/Chess-Position-Notation.md) and submit a hug request.
