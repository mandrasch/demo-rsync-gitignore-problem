
Goal: Rsync everything from remote-site/ to local-site/-folder, except for git tracked folder and files (wp-content/themes/twenttwentyone-child)

```bash
cd demo-rsync-gitignore-problem

rsync -azh --progress --stats remote-site/ local-site/
```

Reset:

- `git clean -fdx`