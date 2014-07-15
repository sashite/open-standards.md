# General Actor Notation <small>Specification and Implementation Guide</small>

A general purpose ASCII-based format for storing actors from abstract strategy games.

<dl class="dl-horizontal">
  <dt>Created</dt>
  <dd><time datetime="2012-05-04T01:23:45Z">4 May 2012</time></dd>

  <dt>Updated</dt>
  <dd><time datetime="2014-07-16T23:42:34Z">16 July 2014</time></dd>

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

With chess variants, it is not always relevant to [identify pieces with a single ASCII character](http://en.wikipedia.org/wiki/Algebraic_notation_(chess)#Naming_the_pieces).  The purpose of this article is to avoid naming ambiguities and collisions between _actors_ on two-player **abstract strategy games**, keeping ASCII character set.

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

The <q>actor</q> term is used for _pawns_, _pieces_ (e.g., queen, rook, bishop, elephant, knight, lance, cannon), _kings_, _stones_, etc.

Keys are composed of two substrings: one for the name of the game and another for the abbreviation of the actor, joined by the "`:`" character; e.g., "`go:stone`" for a Go stone.

The structure of move instances can be unambiguously described with a pattern.

<div class="sub-title">Resource representation</div>

The <abbr title="Backus–Naur Form">BNF</abbr> structure below shows the format of the resource:

    <Actor> ::= <Game> ':' <Abbr>

    <Game>  ::= <text>
    <Abbr>  ::= <text>

### Games

To distinguish the game an actor come from, a first substring is used.  This substring begins with by a letter, and can then contains letters and/or numbers and/or underscores.

For the main chess variants, a single letter abbreviations should be used (e.g., "`j`" for <q>Janggi</q>, "`m`" for <q>Makruk</q>, "`s`" for <q>Shogi</q>, "`w`" for <q>Western</q>, "`x`" for <q>Xiangqi</q>).

For more exotic chess variants, we may use the full name to avoid possible naming collisions (e.g., "`shatar`" for <q>Shatar</q>, "`dai_dai_shogi`" for <q>Dai dai Shogi</q>).

### Abbreviations

To identify actors inside a given game, a second substring is used.

It should contain a letter, which could be followed by letters and/or numbers and/or underscores.  If the actor is a <q>King</q>-like actor, the "`_`" prefix should be used; e.g., "`w:_k`" for a <q>Western King</q>.

For the main chess variants, the following letter abbreviations may be used:

<dl class="dl-horizontal">
  <dt>Janggi actors</dt>
  <dd>
    <ul>
      <li>"<code>_g</code>" (<q>General</q>, called <span lang="ko">楚</span> for Green and <span lang="ko">漢</span> for Red)</li>
      <li>"<code>s</code>" (<q>Guard</q>, called <span lang="ko">士</span>)</li>
      <li>"<code>e</code>" (<q>Elephant</q>, called <span lang="ko">象</span>)</li>
      <li>"<code>r</code>" (<q>Chariot</q>, called <span lang="ko">車</span>)</li>
      <li>"<code>p</code>" (<q>Cannon</q>, called <span lang="ko">砲</span>)</li>
      <li>"<code>m</code>" (<q>Horse</q>, called <span lang="ko">馬</span>)</li>
      <li>"<code>j</code>" (<q>Soldier</q>, called <span lang="ko">卒</span> for Green and <span lang="ko">兵</span> for Red)</li>
    </ul>
  </dd>
  <dt>Makruk actors</dt>
  <dd>
    <ul>
      <li>"<code>_k</code>" (<q>King</q>, called <span lang="th">ขุน</span>)</li>
      <li>"<code>q</code>" (<q>Queen</q>, called <span lang="th">เม็ด</span>)</li>
      <li>"<code>r</code>" (<q>Rook</q>, called <span lang="th">เรือ</span>)</li>
      <li>"<code>b</code>" (<q>Bishop</q>, called <span lang="th">โคน</span>)</li>
      <li>"<code>n</code>" (<q>Knight</q>, called <span lang="th">ม้า</span>)</li>
      <li>"<code>p</code>" (<q>Pawn</q>, called <span lang="th">เบี้ย</span>)</li>
    </ul>
  </dd>
  <dt>Shogi actors</dt>
  <dd>
    <ul>
      <li>"<code>_k</code>" (<q>King</q>, called <span lang="ja">玉</span> for Sente and <span lang="ja">王</span> for Gote)</li>
      <li>"<code>g</code>" (<q>Gold</q>, called <span lang="ja">金</span>)</li>
      <li>"<code>s</code>" (<q>Silver</q>, called <span lang="ja">銀</span>)</li>
      <li>"<code>r</code>" (<q>Rook</q>, called <span lang="ja">飛</span>)</li>
      <li>"<code>b</code>" (<q>Bishop</q>, called <span lang="ja">角</span>)</li>
      <li>"<code>n</code>" (<q>Knight</q>, called <span lang="ja">桂</span>)</li>
      <li>"<code>l</code>" (<q>Lance</q>, called <span lang="ja">香</span>)</li>
      <li>"<code>p</code>" (<q>Pawn</q>, called <span lang="ja">歩</span>)</li>
    </ul>
  </dd>
  <dt>Western actors</dt>
  <dd>
    <ul>
      <li>"<code>_k</code>" (<q>King</q>)</li>
      <li>"<code>q</code>" (<q>Queen</q>)</li>
      <li>"<code>r</code>" (<q>Rook</q>)</li>
      <li>"<code>b</code>" (<q>Bishop</q>)</li>
      <li>"<code>n</code>" (<q>Knight</q>)</li>
      <li>"<code>p</code>" (<q>Pawn</q>)</li>
    </ul>
  </dd>
  <dt>Xiangqi actors</dt>
  <dd>
    <ul>
      <li>"<code>_g</code>" (<q>General</q>, called <span lang="zh">將</span> for Black and <span lang="zh">帥</span> for Red)</li>
      <li>"<code>a</code>" (<q>Advisor</q>, called <span lang="zh">士</span> for Black and <span lang="zh">仕</span> for Red)</li>
      <li>"<code>e</code>" (<q>Elephant</q>, called <span lang="zh">象</span> for Black and <span lang="zh">相</span> for Red)</li>
      <li>"<code>r</code>" (<q>Chariot</q>, called <span lang="zh">車</span> for Black and <span lang="zh">俥</span> for Red)</li>
      <li>"<code>c</code>" (<q>Cannon</q>, called <span lang="zh">砲</span> for Black and <span lang="zh">炮</span> for Red)</li>
      <li>"<code>h</code>" (<q>Horse</q>, called <span lang="zh">馬</span> for Black and <span lang="zh">傌</span> for Red)</li>
      <li>"<code>s</code>" (<q>Soldier</q>, called <span lang="zh">卒</span> for Black and <span lang="zh">兵</span> for Red)</li>
    </ul>
  </dd>
</dl>

### Promoted actors

Through a promotion, the transformation of a promoted actor must be expressed by a different name; e.g., "`w:r`" (Western <q>Rook</q>) for a promoted "`w:p`" (Western <q>Pawn</q>).  If the promoted actor remains the same, the "`+`" char must be used; e.g., "`s:+b`" (Shogi <q>promoted Bishop</q>) for a promoted "`s:b`" (Shogi <q>Bishop</q>).

<span class="label label-info"><q>King</q>-like actors:</span>
Even if this is uncommon, it would be also possible to promote <q>King</q>-like actors; e.g., "`FOO:+_K`" for a <q>promoted King</q> of the Foo variant.

### Naming the sides

The side of actors should be identified thanks to the case sensitivity; that is, based on differing use of _uppercase_ for the bottom actors and _lowercase_ for the top actors letters.

Due to history and culture, there are some differences in the naming of player's side between chess variants.  For example, in Western chess the player who start the game is called <q>White</q> and his opponent is called <q>Black</q>, while in Xiangqi chess the player who start the game is called <q>Red</q> and his opponent is called <q>Black</q>.

For the main chess variants, the following names are used:

|             | Player who moves first         | Player who moves second       |
| ----------- | ------------------------------ | ----------------------------- |
| **Janggi**  | <q>Green</q>                   | <q>Red</q>                    |
| **Makruk**  | <q>White</q>                   | <q>Black</q>                  |
| **Shogi**   | <q><ruby lang="ja">先手<rt lang="en">Sente</rt></ruby></q> (or <q>Black</q>) | <q><ruby lang="ja">後手<rt lang="en">Gote</rt></ruby></q> (or <q>White</q>) |
| **Western** | <q>White</q>                   | <q>Black</q>                  |
| **Xiangqi** | <q>Red</q>                     | <q>Black</q>                  |

To avoid confusion between sides and players, _the player who moves first_ is referred to as **bottom** and _the player who moves second_ is referred to as **top**.  Similarly, the actors that each conducts are called, respectively, <q>the bottom actors</q> and <q>the top actors</q>.

<div class="alert alert-warning">
  <strong>Cross-variant compatibility:</strong>

  <p>
    Unlike other main chess variants, at Janggi the <q>Green</q> side which moves first is traditionally played from the north of the board.  However, in order to keep things consistent and compatible, the sides of Janggi are flipped.
  </p>
</div>

## Example

| Actor                             | Side   | Recommended key       |
| --------------------------------- | ------ | --------------------- |
| Western Pawn, White               | bottom | "`W:P`"               |
| Xiangqi Cannon, Black             | top    | "`x:c`"               |
| Taikyoku Shogi Great Dragon, Gote | top    | "`taikyoku_shogi:gd`" |
| Makruk Bishop, Black              | top    | "`m:b`"               |
| Western King, White               | bottom | "`W:_K`"              |
| Shogi Rook, Sente                 | bottom | "`S:R`"               |
| Shogi promoted Rook, Sente        | bottom | "`S:+R`"              |
| Xiangqi General, Red              | bottom | "`X:_G`"              |
| Janggi General, Red               | top    | "`j:_g`"              |
| Dai Dai Shogi Phoenix, Sente      | bottom | "`DAI_DAI_SHOGI:PH`"  |

* * *

<div class="sub-title">See also</div>

* [An implementation in Ruby](//github.com/sashite/gan.rb)

<div class="sub-title">Informative References</div>

This work is influenced by several documents.

* [General Gameplay Notation](General-Gameplay-Notation)
* [The Universal Shogi Interface](//www.glaurungchess.com/shogi/usi.html)

<div class="sub-title">Contributing</div>

Want to make this page better?  [Make your changes](//github.com/sashite/open-standards.md/edit/master/docs/General-Actor-Notation.md) and submit a hug request.
