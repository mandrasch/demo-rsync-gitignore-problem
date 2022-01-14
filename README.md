
**Goal**

- Rsync everything from remote-site/ to local-site/-folder, 
    - except for git tracked folder and files (wp-content/themes/twenttwentyone-child). 
    - Should be based on local-site/.gitignore (reverse / opposite).


**Current try**

Does not work as well:

```bash
cd demo-rsync-gitignore-problem
git clean -fdx


rsync -azhvv --exclude-from=<(git -C local-site/ ls-files --directory) remote-site/ local-site/

```

(Based on bash processing https://stackoverflow.com/questions/13713101/rsync-exclude-according-to-gitignore-hgignore-svnignore-like-filter-c#comment104587797_50059607 and  https://stackoverflow.com/a/35927535/809939)

--ignored

https://git-scm.com/docs/git-ls-files#Documentation/git-ls-files.txt--i

**Reset experiment**

- `git clean -fdx`

**Already tried:**

Problem is, rsync understands gitignore only partly?

- `rsync -azhvv --filter='+ /.gitignore' --filter='- *' remote-site/ local-site/`
- `--include-from='.gitignore' --exclude='*'` - this does not work, because rsync uses a pattern for it, it doesn't work with `/path` relative from root?
- `rsync -azhvv --exclude='*' --filter='dir-merge,+ /.gitignore' remote-site/ local-site/` 
- `rsync -azhvv --filter='dir-merge,n+ /.gitignore' --filter='- *' remote-site/ local-site/`