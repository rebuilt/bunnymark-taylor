# BunnyMark - Taylor

A [BunnyMark](https://github.com/ianlancetaylor/bunnymark) sprite benchmark for the
[Taylor](https://taylormadetech.dev) game engine, which is built on mruby and raylib.

The game opens an 800x480 window and starts with 1,000 animated characters bouncing
around it. Every frame it adds or removes characters to hold the frame rate near 60,
so the number on screen is a rough measure of how many sprites the renderer can
sustain.

## Files

| File | Description |
| --- | --- |
| `game.rb` | The benchmark: window and audio setup, the `Character` class, and the adaptive main loop. |
| `taylor` | Prebuilt Taylor runtime, used to launch the game. |
| `assets/raybunny.png` | Spritesheet with 24x24 character frames. |
| `assets/kenney_pixel.ttf` | Pixel font used for the on-screen FPS counter. |
| `assets/kenney_kenney-fonts/` | The full Kenney font pack, including its CC0 licence. Only `kenney_pixel.ttf` is used. |
| `assets/backgrounds.png`, `assets/tiles.png`, `assets/map.csv` | Tile and map assets from the same Kenney pack. Not used by the game. |

## Running

From this directory, because asset paths are relative to the working directory:

```sh
./taylor
```

`./taylor game.rb` and `./taylor --entrypoint ./game.rb` work too. The bundled
`taylor` binary is platform-specific; if it does not run on your system, install Taylor
and use its `taylor` command instead.

Taylor opens a window through raylib/GLFW, so a graphical session (X11 or Wayland) is
required. Close the window or press `Esc` to quit.

## How it works

- Each `Character` gets a random colour (green, blue, pink, yellow or cream), a random
  direction of up to 50 pixels per second on each axis, and a 24x24 destination
  rectangle. It bounces off all four window edges.
- The spritesheet stores two animation frames per colour, 24 pixels wide with a
  24-pixel gap: green, blue, pink and yellow on the top row, cream on the second row.
  Each character switches frames at a random interval between 0.25 and 0.75 seconds.
- After the initial 1,000 characters, the loop adds 50 characters whenever the frame
  rate is above 60 and drops 5 when it falls below 55.
- The frame rate and character count are drawn in the top-left corner as
  `FPS: ... / Aliens: ...` using the pixel font at 32px.

The target frame rate is set to the current monitor's refresh rate. The 60 FPS
thresholds are tuned for a 60 Hz display; on a high-refresh monitor the "above 60"
rule will keep adding characters until the frame rate drops below the target.

## Browser builds

The entry point checks `Taylor::Platform.browser?` and uses `Browser.main_loop` when
running in a browser, so `game.rb` can also be built for Taylor's web export instead
of the desktop loop.

## Credits

- Character sprites: [Kenney Pixel Platformer](https://kenney.nl/assets/pixel-platformer), CC0.
- Fonts: [Kenney Fonts](https://kenney.nl/assets/kenney-fonts), CC0.
- BunnyMark itself is a long-running community benchmark, originally written for Flash;
  see the link at the top of this README.

## License

The code in this repository does not specify a licence. The bundled art and font
assets are CC0; see `assets/kenney_kenney-fonts/License.txt`.
