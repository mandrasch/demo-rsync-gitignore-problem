
**Goal**

- Rsync everything from remote-site/ to local-site/-folder, 
    - except for git tracked folder and files (wp-content/themes/twenttwentyone-child). 
    - Should be based on local-site/.gitignore (reverse / opposite).


**Current try**

Does not work as well:

```bash
cd demo-rsync-gitignore-problem
git clean -fdx


rsync -azhvv  --exclude='*' --exclude-from=<(git -C local-site/ ls-files --exclude-standard -oi --directory) remote-site/ local-site/


rsync -azhvv  --exclude='*' --include-from="$(git -C local-site/ ls-files --exclude-standard -oi --directory >.git/ignores.tmp && echo .git/ignores.tmp')" remote-site/ local-site/
```

(Based on https://stackoverflow.com/a/35927535/809939)


**Reset experiment**

- `git clean -fdx`

**Already tried:**

Problem is, rsync understands gitignore only partly?

- `rsync -azhvv --filter='+ /.gitignore' --filter='- *' remote-site/ local-site/`
- `--include-from='.gitignore' --exclude='*'` - this does not work, because rsync uses a pattern for it, it doesn't work with `/path` relative from root?
- `rsync -azhvv --exclude='*' --filter='dir-merge,+ /.gitignore' remote-site/ local-site/` 
- `rsync -azhvv --filter='dir-merge,n+ /.gitignore' --filter='- *' remote-site/ local-site/`