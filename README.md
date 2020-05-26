# Overview

A small X program which facilitates recursively warping the pointer to different quadrants on the screen. The program was inspired by the mousekeys feature of Kaleidoscope, the firmware for the Keyboardio.

# Installation

Requires libxinerama-dev, libxft-dev, libxfixes-dev, libxtst-dev, and libx11-dev on debian. You will need to install the equivalent packages containing the appropriate header files on your distribution.

```
sudo apt-get install libxinerama-dev libxft-dev libxfixes-dev libxtst-dev libx11-dev && make && sudo make install
```

## Getting Started

1. Run `warp -d` 
2. Press M-x (meta is the command key) to activate the warping process.
3. Use u,i,j,k to repeatedly navigate to different quadrants.
4. Press m to click.
6. RTFM
7. For luck.

# Overview

By default `warp` divides the screen into a 2x2 grid. Each time a key
is pressed the grid shrinks to cover the targetted area. Once the pointer
covers the target `m` can be pressed to simulate a mouse click.


E.G

```
         +--------+--------+            +--------+--------+
         |        |        |            |  u |  i |       |
         |   u    |   i    |            |----m----+       |
 M-x     |        |        |     u      |  j |  k |       |
----->   +-----------------+   ----->   +---------+       |
         |        |        |            |                 |
         |   j    |   k    |            |                 |
         |        |        |            |                 |
         +--------+--------+            +--------+--------+
```

## Demo

<img src="demo_warp.gif" height="400px"/>

# Hint Mode

This is an experimental mode which populates the screen with a list of labels and
allows the user to immediately warp the pointer to a given location by pressing
the corresponding key sequence. It is similar to functionality provided by
browser plugins like Vimperator but works outside of the browser and
indiscriminately covers the entire screen. 

## Demo

<img src="demo_hints.gif" height="400px"/>


## Notes

Rather than saturating the screen with labels it is recommended that the user leave
a few gaps and then use the movement keys (hjkl) to move to the
final location. The rationale for this is as follows: 

- **Efficiency**: A large number of labels necessitates the use of longer keysequences (since there are a finite number of two-key sequences) at which point the value of the label system is supplanted by manual mouse movement.

- **Usability**: Packing every inch of the screen with labels causes a loss of context by obscuring UI elements.

- **Performance**: Drawing routines have been optimized with a 20x20 grid in mind (the default) increasing the grid size beyond this may yield sub par performance.

By tweaking hints_nc and hints_nr it should be possible to make most screen
locations accessible with 2-4 key strokes. After a bit of practice this becomes
second nature and is (in the author's opinion) superior to the grid method for
quickly pinpointing text and UI elements.

# Config Options

The following configuration options can be placed in ~/.warprc to modify the behaviour of the program. Each option must be specified
on its own line and have the format 

```
<option>: <value>
```
**grid_nr**: The number of rows in the grid. (default: 2).

**grid_nc**: The number of columns in the grid. (default: 2).

**movement_increment**: The movement increment used for grid and discrete mode. (default: 20).

**grid_up**: Move the entire grid up by a fixed interval. (default: w).

**grid_left**: Move the entire grid left by a fixed interval. (default: a).

**grid_down**: Move the entire grid down by a fixed interval. (default: s).

**grid_right**: Move the entire grid right by a fixed interval. (default: d).

**grid_keys**: A sequence of comma delimited keybindings which are ordered bookwise with respect to grid position. (default: u,i,j,k).

**hint_activation_key**: Activates hint mode. (default: M-z).

**grid_activation_key**: Activates grid mode and allows for further manipulation of the pointer using the mapped keys. (default: M-x).

**discrete_activation_key**: Activate discrete movement mode (manual cursor movement). (default: M-c).

**close_key**: Prematurely terminate the movement session. (default: Escape).

**buttons**: The number of columns in the grid. (default: m,comma,period).

**trigger_mods**: A set of modifiers which, when used in conjunction with the grid keys, immediately activates the grid and moves to the corresponding quadrant. E.G if set to `M-A` then `M-A-u` (where `M` is the windows key) will immediately warp to the first quadrant when pressed. (default: M-C).

**hint_nc**: The number of columns in hint mode. (default: 20).

**hint_nr**: The number of rows in hint mode. (default: 20).

**hint_up**: Moves the cursor up by movement_increment once a label has been selected in hint mode. (default: k).

**hint_left**: Moves the cursor left by movement_increment once a label has been selected in hint mode. (default: h).

**hint_down**: Moves the cursor down by movement_increment once a label has been selected in hint mode. (default: j).

**hint_right**: Moves the cursor right by movement_increment once a label has been selected in hint mode. (default: l).

**hint_bgcol**: The background hint color. (default: #00ff00).

**hint_fgcol**: The foreground hint color. (default: #000000).

**hint_characters**: The set of hint characters used by hint mode. (default: asdfghjkl;'zxcvbm,./).

**grid_col**: The color of the grid. (default: #ff0000).

**grid_mouse_col**: The color of the mouse indicator. (default: #00ff00).

**grid_pointer_size**: The size of the grid pointer. (default: 20).

**grid_line_width**: The size of grid lines. (default: 5).

**grid_activation_timeout**: The length of time before warp automatically deactivates after the last click. 0 disables automatic deactivation entirely. (default: 300).

**discrete_left**: Move the cursor left in discrete mode. (default: h).

**discrete_down**: Move the cursor down in discrete mode. (default: j).

**discrete_up**: Move the cursor up in discrete mode. (default: k).

**discrete_right**: Move the cursor right in discrete mode. (default: l).

**discrete_indicator_color**: The color of the discrete status indicator (default: #00ff00).

**discrete_indicator_size**: The height (in pixels) of the discrete status indicator (default: 30).


# Examples

The following ~/.warprc causes warp to use a 3 by 3 grid instead of the default 2 by 2 grid with u,i,o corresponding to the columns in the top row and n,m,comma corresponding to the columns in the bottom row.

```
grid_nr: 3
grid_nc: 3
grid_keys: u,i,o,j,k,l,n,m,comma
```
# Limitations/Bugs

- No multi monitor support (it may still work by treating the entire display as one giant screen, I haven't tried this). If you use this program and desire this feature feel free to harass me via email or submit a pull request.

- Warp wont activate if the keyboard has already been grabbed by another program (including many popup menus). Using a minimalistic window manager is recommended :P.
See [#3](https://github.com/rvaiya/warp/issues/3) for details.

- This was a small one off c file that ballooned into a small project, I did not originally plan to publish it. Consequently the code is ugly/will eat your face. You have been warned.
