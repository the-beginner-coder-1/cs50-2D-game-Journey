```markdown
# Setting Up Love2D with noVNC on CS50 Codespaces

This guide explains how to set up **Love2D** and the **Pixelbyte Studios Love2D Support** extension inside GitHub CS50 Codespaces using **noVNC and X11** for graphical display rendering directly in your browser.

---

## 1. Install Love2D Engine
Open your CS50 Codespace terminal and install Love2D:

```bash
sudo apt update
sudo apt install -y love

```

Verify the executable path:

```bash
which love
# Output should be: /usr/bin/love

```

---

## 2. Install VS Code Extension

1. Open the **Extensions** panel (`Ctrl+Shift+X`).
2. Search for `Love2D Support`.
3. Install the extension published by **Pixelbyte Studios**.
4. Open **Settings** (`Ctrl+,`) and search for `pixelbyte.love2d.path`.
5. Set the path to:
```text
/usr/bin/love

```



---

## 3. Install Virtual Display & noVNC Requirements

Install Xvfb (Virtual Framebuffer), Fluxbox (Window Manager), and noVNC/websockify:

```bash
sudo apt update
sudo apt install -y xvfb x11vnc fluxbox novnc websockify

```

---

## 4. Set Up Permanent DISPLAY Environment Variable

To ensure VS Code extensions and terminal sessions direct output to the virtual display, append `DISPLAY=:99` to your `.bashrc` file:

```bash
echo "export DISPLAY=:99" >> ~/.bashrc
source ~/.bashrc

```

---

## 5. Launch Virtual Display Services

Run the following background commands to initialize the display server and web proxy:

```bash
Xvfb :99 -screen 0 1024x768x16 &
DISPLAY=:99 fluxbox &
x11vnc -display :99 -forever -nopw -shared -bg &
websockify --web=/usr/share/novnc/ 6080 localhost:5900 &

```

---

## 6. Accessing the Graphical Window via Browser

1. Copy your CS50 Codespace base URL (e.g., `https://<your-codespace-name>.github.dev/`).
2. Construct the 6080 port URL by adding `-6080` and `/vnc.html`:
```text
https://<your-codespace-name>-6080.app.github.dev/vnc.html

```


3. Open this link in a new browser tab and click **Connect**.

---

## 7. Running Love2D Projects

You can run your project in two ways:

* **Via Keyboard Shortcut:** Press `Alt + L` (or `Cmd + L` on macOS) inside VS Code. (I chanced to `Ctrl+K Ctrl+G` because Firefox browser doesn't like Alt shortcuts)
* **Via Terminal:** Navigate to the folder containing `main.lua` and execute:
```bash
love .

```
Your Love2D window will render smoothly inside the open noVNC browser tab.
