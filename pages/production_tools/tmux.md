# intro
## tmux panes
```
ctrl-b %
```
## move between panes
```
ctrl-b <-
```
## Split in other direction
```
ctrl-b "
```
## remove pane
```
exit
```
or
```
ctrl-d
```
## create a new window
```
ctrl-b c
```
## switch next
```
ctrl-b n
```
## switch back
```
ctrl-b p
```
### detatch from session
```
ctrl-b d
```
### attaching to most recent session
```
tmux attach
```
### running s3ssions
```
tmux ls
```
### reattach to a specific session
```
tmux attach -t <session name>
```
### rename a session
```
ctrl-b $
```
### get list of sessions from within tmux
```
ctrl-b s
```
### rename from terminal
```
tmux rename-session -t2 <session> <name>
```
## create new se3ssion with name
```
tmux new -s <session name>
```
## zoom in on pane
```
ctrl-b z
```
## resize
```
ctrl-b :
resize-pane -D
```