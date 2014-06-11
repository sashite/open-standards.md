# General Actor Notation <small>Specification and Implementation Guide</small>

A general purpose ASCII-based format for storing actors from abstract strategy games.

<dl class="dl-horizontal">
  <dt>Created</dt>
  <dd><time datetime="2014-05-04T01:23:45Z">4 May 2014</time></dd>

  <dt>Updated</dt>
  <dd><time datetime="2014-06-01T23:42:34Z">1 June 2014</time></dd>

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

This document proposes a format for representing _actors_ from **abstract strategy games**.

<div class="sub-title">Status of this document</div>

This document is a beta of the GAN specification.

<div class="sub-title">Copyright Notice</div>

The content of this page is licensed under the [Creative Commons Attribution 3.0 License](//creativecommons.org/licenses/by/3.0/), and code samples are licensed under the [Apache 2.0 License](//www.apache.org/licenses/LICENSE-2.0).

* * *

## Introduction

<abbr title="General Actor Notation">GAN</abbr> (<q>General Actor Notation</q>) is a lightweight, <abbr title="American Standard Code for Information Interchange">ASCII</abbr>-based format that gives a consistent and easy way to represent actors from abstract strategy games.

Easy for humans to read and write, and easy for machines to import and export, it is compatible with actors of most abstract strategy board games such as [Draughts](//en.wikipedia.org/wiki/Draughts), [Go](//en.wikipedia.org/wiki/Go_(game)) and the main chess variants, including [<ruby lang="ko">장기<rt lang="en">Janggi</rt></ruby>](//en.wikipedia.org/wiki/Janggi), [<ruby lang="th">หมากรุก<rt lang="en">Makruk</rt></ruby>](//en.wikipedia.org/wiki/Makruk), [<ruby lang="ja">将棋<rt lang="en">Shogi</rt></ruby>](//en.wikipedia.org/wiki/Shogi), [Western](//en.wikipedia.org/wiki/Chess), [<ruby lang="zh">象棋<rt lang="en">Xiangqi</rt></ruby>](//en.wikipedia.org/wiki/Xiangqi).  These properties make GAN an ideal data-interchange format for storing actors of most abstract strategy board games.

### Notational conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](//tools.ietf.org/html/rfc2119).

#### Naming the actors

The <q>actor</q> term is used for _pawns_, _pieces_ (e.g., queen, rook, bishop, elephant, knight, lance, cannon), _kings_, _stones_, etc.

Keys are how actors of games are identified.  A key might look like "`2:bc096c4c7f48fc5c4c162555e4df98169e204aea:t:x:r`".  One could reasonably expect such a key to identify a Black Rook from Xiangqi game.  Colons "`:`" doesn't have any special meaning, as far as GAN is concerned, but using a separator is a common approach to organize their properties.

## Specification goals

A specification for a general actor notation MUST observe the following criteria:

1. The details of the system MUST be publicly available and free of unnecessary complexity.
2. The details of the system MUST be non-proprietary so that users and software developers are unrestricted by concerns about infringing on intellectual property rights.
3. The system MUST work for a variety of programs.  The format SHOULD be such that it can be used by abstract strategy board game database programs, abstract strategy board game server programs, and abstract strategy board game playing programs without being unnecessarily specific to any particular application class.
4. The system MUST handle abstract strategy board games which are playable by multiple sides.
5. The system MUST be easily expandable and scalable.  Examples: new actors, new games, new moves, new rules, etc.
  * The system MUST be cross-variants.
    It SHOULD be able to represent patterns of actors where sides MAY play
    different abstract strategy board games such as Janggi, Go, Draughts, Shogi, Western, Xiangqi.
  * The system MUST handle pattern of move of boards of any dimension; e.g., 2D, 3D.
  * The system MUST handle pattern of move of boards of any size; e.g., 8x8, 9x9, 9x9x9.
  * The system MUST be able to fully describe actor patterns according to
    a concerned Laws of Chess and the given chessboard structure.
6. Finally, the system SHOULD handle the same kinds and amounts of data that are already handled by existing chess software and by print media.

## How to serve GAN

The use of the file suffix "`.gan`" is RECOMMENDED for files containing GAN data.

When serving GAN over HTTP, the media type "`application/vnd.gan`" is RECOMMENDED.

## <span id="resource">Notation for actors</span>

The structure of move instances can be unambiguously described with a pattern.

<div class="sub-title">Resource representation</div>

The <abbr title="Backus–Naur Form">BNF</abbr> structure below shows the format of the resource:

    <Actor>                 ::= <Dimension> ':' <Gameplay> ':' <Side> ':' <Variant> ':' <Name>
    <Dimension>             ::= <unsigned integer>
    <Gameplay>              ::= <text>
    <Side>                  ::= <text>
    <Variant>               ::= <text>
    <Name>                  ::= <text>

    <unsigned integer>      ::= <digit excluding zero>
                              | <unsigned integer> <digit>
    <digit>                 ::= <digit excluding zero>
                              | '0'
    <digit excluding zero>  ::= '1'
                              | '2'
                              | '3'
                              | '4'
                              | '5'
                              | '6'
                              | '7'
                              | '8'
                              | '9'

## Example

Here is the GAN of a famous actor:

    2:t<self>_&_^remove[-1,0]1/t=_@f+all~_@an_enemy_actor+all%[self]. t<self>_&_^remove[0,-1]1/t=_@f+all~_@an_enemy_actor+all%[self]. t<self>_&_^remove[0,1]1/t=_@f+all~_@an_enemy_actor+all%[self]. t<self>_&_^remove[1,0]1/t=_@f+all~_@an_enemy_actor+all%[self]. t<self>_&_^shift[-1,0]_/t=_@f+all~_@f+all%[self]. t<self>_&_^shift[-1,0]_/t=_@f+all~_@f+all%[self]; t<self>_&_^remove[-1,0]1/t=_@f+all~_@an_enemy_actor+all%[self]. t<self>_&_^shift[0,-1]_/t=_@f+all~_@f+all%[self]. t<self>_&_^shift[0,-1]_/t=_@f+all~_@f+all%[self]; t<self>_&_^remove[0,-1]1/t=_@f+all~_@an_enemy_actor+all%[self]. t<self>_&_^shift[0,1]_/t=_@f+all~_@f+all%[self]. t<self>_&_^shift[0,1]_/t=_@f+all~_@f+all%[self]; t<self>_&_^remove[0,1]1/t=_@f+all~_@an_enemy_actor+all%[self]. t<self>_&_^shift[1,0]_/t=_@f+all~_@f+all%[self]. t<self>_&_^shift[1,0]_/t=_@f+all~_@f+all%[self]; t<self>_&_^remove[1,0]1/t=_@f+all~_@an_enemy_actor+all%[self].:top:xianqi:rook

Where:

 * the context is a two-dimensional board;
 * the gameplay looks like Western and Xiangqi Rooks;
 * the actor belongs to the player who moves second;
 * the variant is Chinese chess;
 * the name of the actor is Rook.

* * *

<div class="sub-title">Informative References</div>

This work is influenced by several documents.

* [General Gameplay Notation](General-Gameplay-Notation)

<div class="sub-title">Contributing</div>

Want to make this page better? [Make your changes](//github.com/sashite/open-standards.md/edit/master/docs/General-Actor-Notation.md) and submit a hug request.
