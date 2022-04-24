<h1>GB Studio Tutorial - Animated Tiles</h1>

<img src="/help/AnimatedTiles.1.4.gif" />

This is a basic interactive how-to tutorial for animated background tiles in GBStudio 3. The source code was built using with GBS 3.0.3.

This tutorial based on NalaFala (Yousurname)'s <a href="https://gist.github.com/Y0UR-U5ERNAME/b1cdf7f3afff6c14dfa557dc5e6b1572">Background Tile Swapping Guide for GB Studio 3</a><br/>
But I've dumbed it down a bit for people new to GBVM, like me :)

### GB Studio Tutorials:

- [1.0 - Animated Tiles](https://github.com/phinioxGlade/gbstudio-3-animated-tile-tutorial) &lt; This Tutorial
- [2.0 - Background Sprites](https://github.com/phinioxGlade/gbstudio-background-sprites-tutorial)

### Play in your Browser
You can play the interactive version at <a href="https://phinioxglade.itch.io/gbstudio-3-animated-tile-tutorial">https://phinioxglade.itch.io/gbstudio-3-animated-tile-tutorial</a>

<h3>We will cover the following 5 key ascepts of tile swap: </h3>
<ul>
  <li><a href="#10-basic-tile-swapping">1.0 - Basic Tile Swapping</a></li>
  <li><a href="#11-animating-a-tile">1.1 - Animating a tile</a></li>
  <li><a href="#12-impact-of-replacing-tile">1.2 - Impact of replacing tile</a></li>
  <li><a href="#13-update-a-single-tile">1.3 - Update a single tile</a></li>
  <li><a href="#14-update-a-multiple-unique-tile">1.4 - Update a multiple unique tile</a></li>
</ul>
  
<h3>Additionally we will cover GBVM script features: </h3>
<ul>
  <li>Accessing global variables</li>
  <li>Set varaibles</li>
  <li>Adding a value to a variable</li>
</ul>

### Useful Resources

- <a href="https://github.com/untoxa/gbvm/blob/master/include/vm.i">GBVM source file</a>
- <a href="https://gist.github.com/pau-tomas/92b0ad77506088d184a654af226f5b7d">GBVM Command Reference</a>
- <a href="https://gbstudiocentral.com/tips/understanding-gbvm/">GB Studio Central - Understanding GBVM</a>
- <a href="https://docs.google.com/spreadsheets/d/1d2F5hSEMt6nkacw-qVnYlT3IPHqmCCaLFhRboC5xxc0/edit#gid=0">GB Studio Plug-In Database</a>

<div>
<h3>1.0. Basic Tile Swapping</h3>
<p>
  We can swap tile with another tile using GBVM scripts.<br/>
  There are 2 commands which can do this: <br/>
  <ul>
    <li>VM_REPLACE_TILE</li>
    <li>VM_REPLACE_TILE_XY</li>
  </ul>
  VM_REPLACE_TILE_XY is the easier to use. So will focus on it.<br/>
</p>
<p>
  The function VM_REPLACE_TILE_XY has 5 paramaters:
  <ol>
    <li><strong>X</strong> = position of the tile to be swapped, e.g. 3</il>
    <li><strong>Y</strong> = position of the tile to be swapped, e.g. 8</li>
    <li><strong>TILEDATA_BANK</strong> = not sure but it appears to be tiles in VRAM. just use ___bank_tileset_0</li>
    <li><strong>TILEDATA</strong> = is the tileset reference, e.g. _tileset_0</li>
    <li><strong>START_IDX</strong> = is the tile index in the tileset we want to swap tile X Y with, the value between 0 to 255</li>
  </ol>
  
  In this tutorial we have two tilesets:
  <ol>
    <li>_tileset_0 = !letters</li>
    <li>_tileset_1 = !waterfall</li>
  </ol>
  Tilesets are assigned in alphabetical order of the background name<br/>
  
  <i>Future versions of GBStudio will allow you to get the TILEDATA_BANK and TILEDATA from within the UI</i>
</p>
  Now for the fun bit :)<br/>
  We will swap the tile at X=14 Y=9 with tile in _tileset_0 at index 57, tile X=17 Y=2 in the !letters scene
  
  <img src="/help/Slide1.png" width="50%" />
</p>

  <h4>GBVM Script Example</h4>
<pre>
; start of GBVM script

; lines starting with ; are comments and are ignored by the compiler

; push the START_IDX value we want onto the stack, then it can be accessed by the alias .ARG0
; This take up memory, be sure to release it when done
VM_PUSH_CONST 57

; call the swap function by passing the START_IDX using .ARG0 alias
VM_REPLACE_TILE_XY 14, 9, ___bank_tileset_0, _tileset_0, .ARG0

; free memory assigned to .ARG0
VM_POP 1

; end of GBVM script
</pre>

And just like that we have swapped the tile
</div>
</p>
 Now we will replace the same tile with tile in !waterfall tileset at index 2
 <img src="/help/Slide2.png" width="50%" />
</p>

<h4>GBVM Script Example</h4>
<pre>
; start of GBVM script
VM_PUSH_CONST 2
VM_REPLACE_TILE_XY 14, 9, ___bank_tileset_0, _tileset_1, .ARG0
VM_POP 1
; end of GBVM script
</pre>

And just like that we have swapped the tile
</div>

<div>
  <h3>1.1. Animating a tile</h3>

  <img src="/help/AnimatedTiles.1.2.gif" />
  
  <p>
    By using a loop we can animate a tile.<br/>
    This done by referencing the next tile in the tileset.<br/>
    We need to store which tile index we want to replace.<br/>
    In this example are using a global variable called:<br/>
    <b>Loop_Index</b><br/>
    Each time we loop, add 1 to Loop_Index.<br/>
    When Loop_Index becomes greater than 255 it will wrap back to 0.<br/>
    So we dont need to check for index of out bounds in this case.<br/>
  </p>

  <p>
    To use global variables in GBVM you must prefixing <b>VAR_</b> to your variable name in uppercase:<br/>
    <pre>VAR_<VariableName></pre>
    <b>Example</b>
    <pre>VAR_LOOP_INDEX</pre>
  </p>

  <p>
    So you use the Loop_Index in <b>VM_REPLACE_TILE_XY</b> we need to reference like so:
    <pre>VM_REPLACE_TILE_XY 14, 9, ___bank_tileset_0, _tileset_0, VAR_LOOP_INDEX</pre>
  </p>
</div>

<div>
  <h3>1.2. Impact of replacing tile</h3>
  <p>
    When you change a tile all tiles of the same memory address are changed.
  </p>
</div>

<div>
  <h3>1.3. Update a single tile</h3>
  <img src="/help/AnimatedTiles.1.3.gif" />
  <p>
    If you want to change a single tile, it must be unique in the source artwork.<br/>
    Tiles 1, 2 and 3 are unqiue. Even if tile 1 is replaced with 2 or 3 from the tileset, it started with a different memory address. So it remains unique.<br/>
  </p>
</div>

<div>
  <h3>1.4. Update a multiple unique tile</h3>

  <img src="/help/AnimatedTiles.1.4.gif" />
  
  <p>
    If you want to animate more than unique tiles, you need to replace each individually.<br/>
    In this example we will use the staggered animation to highlight the effect.
  <p>
  
  <h4>How to use VM_RPN</h4>
  <p>
    We can use <b>VM_RPN</b> to calculate the offset, which has 4 paramaters:<br/>
  </p>
  <ol>
    <li>
      .R_REF &lt;VAR_NAME&gt;<br/>
      &lt;VAR_NAME&gt; is the variable input, example <b>.R_REF</b> VAR_LOOP_INDEX	
    </li>
    <li>
      &lt;.R_Type&gt; &lt;Opperator&gt;<br/>
      R_Type: The value type, .R_INT8 or .R_INT16, there might be more options
      &lt;Value&gt;: input value to be used, example .R_INT8 27
    </li>
    <li>
      .R_OPERATOR &lt;Opperator&gt;<br/>
      &lt;Opperator&gt; which math function, example .R_OPERATOR .ADD, there is many, refer to the <a href="https://github.com/untoxa/gbvm/blob/master/include/vm.i">GBVM reference</a>
    </li>
    <li>
      .R_STOP<br/>
      guessing it stops the function
    </li>
  </ol>
  
  <h4>GBVM Script Example</h4>
  <pre>
; start of GBVM script
; calculate the next tile index by added 1 to 
VAR_LOOP_INDEX
VM_RPN
    .R_REF      VAR_LOOP_INDEX
    .R_INT8     1
    .R_OPERATOR .ADD
    .R_STOP

; replace the tile with using the calcaluted offset index
VM_REPLACE_TILE_XY      14, 9, ___bank_tileset_0, _tileset_0, .ARG0

; if we use VAR_LOOP_INDEX we would added 2
; if we use last variable calculated, .ARG0, we can add 1 since it already 1
; this shows you can reuse .ARG0
VM_RPN
    .R_REF      .ARG0
    .R_INT8     1
    .R_OPERATOR .ADD
    .R_STOP

VM_REPLACE_TILE_XY      17, 9, ___bank_tileset_0, _tileset_0, .ARG0

VM_POP 1
; end of GBVM script
  </pre>
  
</div>

Hopefully this turtorial is useful to you
