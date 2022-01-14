
Goal: Rsync everything from remote-site/ to local-site/-folder, except for git tracked folder and files (wp-content/themes/twenttwentyone-child). Should be based on local-site/.gitignore (reverse / opposite).

```bash
cd demo-rsync-gitignore-problem

rsync -azhvv --filter=':- .gitignore' remote-site/ local-site/
```

Reset:

- `git clean -fdx`