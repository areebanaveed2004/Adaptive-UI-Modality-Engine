# Adaptive UI Modality Engine

Adaptive UI Modality Engine is a dependency-free web app built with plain HTML, CSS and JavaScript. It detects how a user is interacting with the page (mouse, touch, keyboard or voice) and adapts the interface to that input modality. It also includes a mouse movement tracker and voice-controlled theme switching.

Most interfaces assume one kind of input. This one notices which input you are using and changes itself.

## What it does

1. The page listens for mouse, touch, keyboard and voice input.
2. A badge at the top always shows the active modality, and the interface adapts to it (button size, focus ring).
3. A mouse movement tracker records pointer activity and shows live statistics and a speed-based trail.
4. Saying a colour out loud changes the theme of the whole page.

## Quick start

No build step and no install.

```bash
git clone https://github.com/areebanaveed2004/adaptive-ui-modality-engine.git
cd adaptive-ui-modality-engine
```

Then either open `adaptive-ui-modality-engine.html` in Chrome or Edge, or serve the folder locally:

```bash
python -m http.server 8000
# visit http://localhost:8000/adaptive-ui-modality-engine.html
```

Allow microphone access when the browser asks. If voice recognition does not start when the file is opened directly, use the local server option.

## How to use it

1. Move the mouse, tap the screen or press Tab and watch the badge and button sizes change.
2. Click **Interactive Action Button** to see which modality triggered it.
3. Click **Secondary Action** to toggle a manual compact-size override.
4. Move and click inside the dashed box to use the mouse tracker.
5. Click **Start Listening** and say a colour to change the theme.
6. Click **Download CSV** to export the tracker data, or **Clear** to reset it.

## Adaptive modalities

| Modality | Trigger | Adaptation |
|---|---|---|
| Mouse (fine pointer) | `mousemove` | Compact buttons, precise layout |
| Touch (coarse pointer) | `touchstart` | Enlarged buttons and touch targets |
| Keyboard | `Tab` key | Thick orange focus ring |
| Voice | A recognised voice command | Medium-large buttons, accent-colour focus ring |

Mouse events that touch screens emulate after a tap are ignored for 800 ms, so a tap does not flip the badge back to Mouse.

## What is measured

The tracker records pointer activity (mouse, and touch drag) inside the tracking box.

| Metric | Meaning | Why it matters |
|---|---|---|
| Position (x, y) | Current pointer position inside the box | Shows where the pointer is |
| Speed (px/s) | Distance moved divided by time between events | Shows how fast or hesitant the movement is |
| Distance (px) | Total pixels the pointer has travelled | Directness of the movement |
| Move events | Number of pointer movements | Amount of activity |
| Clicks | Number of presses inside the box | Interaction count |
| Time in area (s) | Time the pointer has spent inside the box | Attention on the area |

The trail drawn on the canvas gets thicker and more opaque the faster the pointer moves, and each click is drawn as a red dot. Every tenth move event and every click is saved for export, up to 5,000 samples.

## Voice theme control

Click **Start Listening** and say one of these words:

| Say | Result |
|---|---|
| white | White theme |
| black | Black theme |
| red, blue, green, yellow, orange, purple, pink | Matching colour theme |
| grey or gray | Grey theme |
| dark, reset or default | Back to the original dark theme |

The theme changes the background, cards and accent colour. The text colour switches between light and dark automatically so it stays readable. Colour swatches are included as a fallback for browsers without speech recognition.

## How it works

- **Modality detection:** `setModality()` swaps a `mode-*` class on `<body>`, and CSS custom properties (`--btn-padding`, `--btn-font-size`, `--focus-ring`) change with each class.
- **Theming:** each colour maps to `--bg`, `--card` and `--accent` tokens. The text colour is chosen from the background's relative luminance.
- **Tracker:** `pointermove` events update the statistics and draw line segments on a canvas. Samples are stored in memory for CSV export.
- **Voice:** the Web Speech API runs in continuous mode and looks for colour keywords in each transcript.

## Browser support

- Chrome and Edge are the intended targets, including voice.
- Other browsers should run everything except voice recognition, where the colour swatches can be used instead. This has not been tested.

## Project structure

```
adaptive-ui-modality-engine/
├── adaptive-ui-modality-engine.html   # page, styles and script in one file
└── README.md
```

## Limitations and next steps

- Voice recognition needs a browser with the Web Speech API and, in Chrome, an internet connection.
- Tracker data is kept in memory and is lost on reload, so export it if you need it.
- Voice matching is keyword-based, so the colour word needs to be spoken clearly.
- Possible extensions: a heatmap view of the tracked movement, more voice commands (font size, layout), and remembering the chosen theme between visits.

## Author

Areeba Naveed
