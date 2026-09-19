# WORKLOG — TestForge Automation one-page site

One line per slice: what I did -> the command I ran -> what it actually printed.

- Listed the project folder before writing anything -> `Get-ChildItem -LiteralPath "C:\Users\datta\Downloads\TestForge" -Recurse -Force` -> 5 files: logo.jpeg, logo-dark.jpeg, profile.jpeg, speaking.jpeg, working.jpeg. Shot-1..Shot-6 absent.
- Searched for the brief's CONTACT DETAILS block -> `grep "CONTACT DETAILS"` over C:\Users\datta\Downloads -> No files found. Brief text ended mid-LOOK at "A service shot already carries its own headline and their log". Name, WhatsApp and email not held; asked the member and waited.
- Re-checked the folder on request -> `Get-ChildItem ... -Force` -> Shot-1.jpeg, Shot-2.jpeg, Shot-3.jpeg, Shot-4.jpeg, Shot-5.jpeg, Shot-6.jpeg now present (11 images total, capital S).
- Contact details supplied in chat -> name "Amitesh Datt", WhatsApp "+918892401870", email "dattamitesh93@gmail.com".
- Measured image geometry -> `System.Drawing.Image::FromFile` per file -> speaking 1376x768, working 896x1200, logo/logo-dark/profile/Shot-1..6 all 1024x1024.
- Measured logo content bounds so the header logo is not a tiny mark in a white square -> pixel bounding box at threshold 40 -> logo.jpeg background 250,250,250; content x 126-900, y 382-640 (X 12.3%-87.9%, Y 37.3%-62.5%, centred). Header logo is a 150x40 `object-fit: cover` crop with `mix-blend-mode: multiply`.
- Wrote index.html and styles.css (Sora + Inter from Google Fonts, five named hex colours).
- Rendered to look at it -> Edge `--headless=new --window-size=1440,900 --screenshot` -> hero rendered; found the logo wordmark duplicated as text next to the logo, and the accent span covering two words. Removed the duplicate text; accent changed to one word ("Friday.").
- Full-page render at 1440 -> `--window-size=1440,6600 --screenshot` -> all six sections in the required order with images present.
- True mobile render -> Node CDP `Emulation.setDeviceMetricsOverride` 390x844 -> `{"vw":390,"scrollW":390,"scrollH":7794,"overflow":[]}` — no horizontal overflow; the earlier 390px window had been clamped by Windows to a 481px layout, which is why text looked cut off.
- Full-page screenshots showed blank images -> CDP `Page.captureScreenshot` with `captureBeyondViewport` does not rasterise offscreen images -> captured with a viewport as tall as the page instead; probe then reported `unloaded: []` at both widths.
- Capped the mobile hero image with `min(60svh, 560px)` so a tall capture viewport cannot stretch it.
- Found the about photo stretched -> measured boxes -> before: `.about-media img` 365x1200 (the `height="1200"` attribute was not overridden, so `object-fit: cover` cropped it to a strip); after adding `height: auto;` -> 365x489, grid 570.
- Corrected the shot gallery captions -> the first image had been captioned as a test-flow scene but Shot-1.jpeg is a portrait -> reordered to Shot-4, Shot-2, Shot-5, Shot-3, Shot-6, Shot-1 with captions matching each picture.
- Final render -> CDP at 1440x900 and 390x844 -> desktop `{"vw":1440,"scrollW":1440,"scrollH":5502,"overflow":[],"unloaded":[]}`, mobile `{"vw":390,"scrollW":390,"scrollH":7817,"overflow":[],"unloaded":[]}`.
- Created the GitHub repository and pushed -> `gh repo create TestForgeAutomation --public --source . --remote origin --push` -> see REPORT.md for the URL printed.
