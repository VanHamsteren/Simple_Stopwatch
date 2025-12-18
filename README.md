# Simple Stopwatch

A modern, responsive stopwatch web app built with **HTML**, **CSS**, and **vanilla JavaScript**.  
The stopwatch displays minutes, seconds, and centiseconds, supports **start**, **stop**, **reset**, and **lap** actions, and features a sleek dark theme with a digital-style font.

## Features

- Digital stopwatch in `MM:SS:CC` format (centiseconds).
- Start, pause, and reset the timer.
- Lap functionality:
  - Lap times are listed below the stopwatch.
  - The number of laps is tracked and displayed.
- Responsive design for desktop and mobile.
- Modern UI:
  - Dark theme.
  - Glassmorphism-style card.
  - Digital Google Font (Orbitron).

## Technologies

- **HTML5**: Page structure.
- **CSS3**: Layout, theming, responsive design, button effects.
- **JavaScript (ES6)**: Timing logic, DOM updates, and event handling.
- **Google Fonts**: Orbitron font for the timer display.

## Files / Structure

In the current version, everything is contained in a single HTML file:

```text
.
└── index.html    # Contains HTML, embedded CSS, and JavaScript
```

You can later split this into `index.html`, `style.css`, and `script.js` if you want a more professional structure.

## Usage

1. Save the code as `index.html` in a folder of your choice.
2. Open `index.html` in your browser (Chrome, Firefox, Edge, etc.).
3. Control the stopwatch:
   - **Start**: Start or resume the timer.
   - **Stop**: Pause the timer.
   - **Reset**: Reset the timer to `00:00:00` and clear all laps.
   - **Lap**: Add the current time to the lap list (only when the stopwatch is running).

## Core Code Concepts

### Timing

The stopwatch uses milliseconds and `setInterval` to increment time every 10 ms:

```js
let tRunning = false;
let tInterval;
let tElapsed = 0; // in milliseconds

function tUpdate(ms) {
    const totalSeconds = Math.floor(ms / 1000);
    const M  = Math.floor(totalSeconds / 60);
    const S  = totalSeconds % 60;
    const CS = Math.floor((ms % 1000) / 10); // centiseconds

    const tText =
        `${String(M).padStart(2, '0')}:` +
        `${String(S).padStart(2, '0')}:` +
        `${String(CS).padStart(2, '0')}`;

    tStatus.textContent = tText;
    return tText;
}

function timerStart() {
    if (!tRunning) {
        tRunning = true;
        tInterval = setInterval(() => {
            tElapsed += 10;
            tUpdate(tElapsed);
        }, 10);
    }
}
```

### Lap Function

Each lap adds an item to the list with an incrementing lap number and the captured time:

```js
let lapIndex = 0;

function timerLap() {
    if (!tRunning) return;

    const t = tUpdate(tElapsed);
    lapIndex++;

    const li        = document.createElement('li');
    const indexSpan = document.createElement('span');
    const timeSpan  = document.createElement('span');

    indexSpan.className = 'index';
    indexSpan.textContent = `Lap ${lapIndex}`;
    timeSpan.className = 'time';
    timeSpan.textContent = t;

    li.appendChild(indexSpan);
    li.appendChild(timeSpan);
    lapsList.prepend(li); // newest lap at the top

    updateLapCount();
}
```

## Installation & Development

- No external build steps are required.
- Simply opening the file in a browser is enough to run the app.
- For development, you may use a simple static server (for example `npx serve` or your IDE’s built‑in web server), but this is optional.

## Possible Enhancements

- Keyboard shortcuts (e.g. space = start/stop, L = lap).
- Export lap times to CSV.
- Theme switcher (dark/light mode).
- Local storage (localStorage) to persist lap times between sessions.
