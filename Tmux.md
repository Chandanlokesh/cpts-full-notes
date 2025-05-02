Nice! You’ve gathered solid raw notes. Let me help you structure and clean them up so they’re easy to revise and understand later.

---

## 🧠 **Tmux Notes (Terminal Multiplexer)**

### 🔧 Installation

```bash
sudo apt install tmux -y
```

---

### 🚀 Starting Tmux

```bash
tmux         # Start a new tmux session
```

To detach from a session:

- `Ctrl + b`, then `d` (denoted as `C-b d`)
    

To re-attach:

```bash
tmux a       # Attach to the most recent session
tmux a -t <name>  # Attach to a named session
```

---

### 🧱 Tmux Structure (3 Layers)

#### 1. **Session Layer**

- `tmux new -s <name>` → Create a named session
    
- `tmux ls` → List active sessions
    
- `tmux a` → Attach to the most recent session
    
- `tmux a -t <name>` → Attach to a specific session
    
- `tmux kill-session` → Kill the recent session
    
- `tmux kill-session -t <name>` → Kill a specific session
    

#### 2. **Window Layer**

- `C-b c` → Create new window
    
- `C-b n` → Next window
    
- `C-b ,` → Rename current window
    
- `C-b w` → Show list of windows
    
- `C-b &` → Kill current window
    

#### 3. **Pane Layer**

- `C-b %` → Vertical split
    
- `C-b "` → Horizontal split
    
- `C-b <arrow keys>` → Move between panes
    
- `C-b q` → Show pane numbers
    
- `C-b q + <number>` → Switch to specific pane
    
- `C-b Ctrl + <arrow>` → Resize pane (small adjustment)
    
- `C-b Alt + <arrow>` → Resize pane (larger adjustment)
    
- `C-b Alt + <number>` → Create pre-defined layout (depends on setup)
    
- `C-b x` → Kill the current pane
    

---

### 💣 Kill Everything

```bash
tmux kill-server  # Destroys all sessions
```

---

### 📋 Copy Mode (like Vim)

#### Enable in config:

Edit your Tmux config file:

```bash
vim ~/.tmux.conf
```

Add:

```tmux
set -g mouse on
setw -g mode-keys vi
```

#### Usage:

- `C-b [` → Enter copy mode
    
    - Use `space` to start selection
        
    - Move with `h`, `j`, `k`, `l` (Vim keys)
        
- `C-b ]` → Paste copied text
    

---

Want me to export this as a PDF or Markdown file for easier reference later?