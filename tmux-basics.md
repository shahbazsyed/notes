# tmux Basics

**I. Core Concepts**

*   **tmux Objects:** tmux consists of three primary hierarchical objects:
    *   **Sessions:** The topmost layer, representing a collection of one or more windows managed as a single unit.
        *   You can have multiple sessions open, but you can only be attached to one session at a time.
        *   Each session has a single active window.
    *   **Windows:** Represent a single view in tmux and can contain one or more panes.
    *   **Panes:** Subdivisions within a window, allowing you to see multiple terminal outputs at once.

**II. Keybindings & Prefix Key**

*   **Prefix Key:** All tmux commands require a prefix key. By default, this is `Ctrl b`.

**III. Sessions**

*   **Create a New Named Session & Attach:**
    *   `tmux new -s my-session`: Creates a new session named "my-session" and attaches to it.
*   **Attach to a Specific Session:**
    *   `tmux attach -t my-session`: Attaches to an existing session named "my-session".
*   **List Sessions:**
    *  `tmux ls`: Shows all currently opened tmux sessions.

**IV. Windows**

*   **Create a New Window:**
    *   `Ctrl b c`: Creates a new window and makes it the current active window in the current session.
*   **Switch Between Windows:**
    *   `Ctrl b 0`, `Ctrl b 1`, `Ctrl b 2`...: Switch to the window with the corresponding number.
*   **Preview All Windows and Sessions:**
    *   `Ctrl b w`: Opens a preview of all windows from all sessions and allows you to attach to any of them by pressing `Enter`.
*   **Rename Current Window:**
    *   `Ctrl b ,`: Renames the current window to the name specified by the user.

**V. Panes**

*   **Split Panes:**
    *   `Ctrl b %`: Splits the current window into two panes of equal width (vertically).
    *   `Ctrl b "`: Splits the current window into two panes of equal height (horizontally).
*   **Navigate Between Panes:**
    *   `Ctrl b <arrow key>`: Moves focus to the pane in the direction of the arrow key (up, down, left, right).
*   **Switch Between Panes by Number:**
    *   `Ctrl b q`: Shows numbers for all panes. Then, type the number of a pane to switch focus to it.
*   **Toggle Pane Zoom:**
    *   `Ctrl b z`: Makes the current pane occupy the full width of the terminal window. Repeating this shortcut restores the original layout.
*   **Convert Pane to a New Window:**
    *  `Ctrl b !`: Detaches the current pane and turns it into a new window.

**VI. Other Useful Commands**

*   **Enter Command Mode:**
    *   `Ctrl b :`: Enters tmux command mode, where you can type tmux commands directly. For example, to rename the current window use `Ctrl b :` and then type `rename-window "new name"` and press enter.
*  **Detach from Current Session**
   *  `Ctrl b d`: Detaches from the current session, leaving it running in the background.
*  **Kill a Session**
   *   `Ctrl b &`: Kills the current session. You will be prompted to confirm the action.