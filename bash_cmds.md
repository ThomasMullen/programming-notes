```bash
# find all recursive directories
ls -Rd */**
```

```bash
# find all directories with a keyword "_run"
find -name \*_run\*
```



```bash
# find all directories to a max and min depth
find . -maxdepth 2 -mindepth 2 
find . -mindepth 4 -type d
```

```bash
# view the file
cat htc-docker-test 
```

```bash
# view the file
cat htc-docker-test 
```

link an alias for my_area compared with full dir path
```bash
ln -s my_area nfs/.../thomas
```

Tmux preventing a remote session from crashing when logging remotely
```bash
# ssh into machine
# ...

# create a session
tmux new -s [test]

# close window
# ...

# reopen and ssh back into window
tmux attach -t [test]
```
Adding venv to jupyter notebook
```bash
pip install --user ipykernel
pip install jupyter
ipython kernel install --user --name=fimpy_env

```

Make a link into a qr code
```bash
brew install qrencode
qrencode --help
qrencode -t ANSI -o - "https://www.youtube.com/watch?v=9CxvtaGS4yY&list=LL&index=2"
# qrencode -t ANSI -o - [link]

```
