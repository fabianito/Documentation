
It depends on the version of tmux. When tmux mouse is on then the mouse selections will not span panes and will be copied into tmux's selection buffer. When tmux mouse is off (as it is in the description) then the mouse selection will be native X (and span panes).

I add the following to my ~/.tmux.conf. It will enable CTRL+b M (to turn tmux mouse on) and CTRL+b m (to turn tmux mouse off).

For tmux 1.x - 2.0

### Toggle mouse on
bind-key M \
  set-window-option -g mode-mouse on \;\
  set-option -g mouse-resize-pane on \;\
  set-option -g mouse-select-pane on \;\
  set-option -g mouse-select-window on \;\
  display-message 'Mouse: ON'

### Toggle mouse off
bind-key m \
  set-window-option -g mode-mouse off \;\
  set-option -g mouse-resize-pane off \;\
  set-option -g mouse-select-pane off \;\
  set-option -g mouse-select-window off \;\
  display-message 'Mouse: OFF'
For tmux 2.1+

### Toggle mouse on
bind-key M \
  set-option -g mouse on \;\
  display-message 'Mouse: ON'

### Toggle mouse off
bind-key m \
  set-option -g mouse off \;\
  display-message 'Mouse: OFF'
