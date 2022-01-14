
**Goal**

- Rsync everything from remote-site/ to local-site/-folder, 
    - except for git tracked folder and files (wp-content/themes/twenttwentyone-child). 
    - Should be based on local-site/.gitignore (reverse / opposite).


**Current try**

Does not work as well:

```bash
cd demo-rsync-gitignore-problem
git clean -fdx


rsync -azhvv --exclude-from=<(git -C local-site/ ls-files --directory) --exclude-from=<(git -C local-site/ ls-tree -rt HEAD | awk '{if ($2 == "tree") print $4;}') remote-site/ local-site/
```

Problem: 
- git ls-files is handy, but does only work for files (not directories)
```
.gitignore
wp-content/themes/twentytwentyone-child/functions.php
```
- git ls-tree shows directories, but all directories instead of last level (that's what we would need)
```
local-site
local-site/wp-content
local-site/wp-content/themes
local-site/wp-content/themes/twentytwentyone-child
```

(Based on bash processing https://stackoverflow.com/questions/13713101/rsync-exclude-according-to-gitignore-hgignore-svnignore-like-filter-c#comment104587797_50059607 and  https://stackoverflow.com/a/35927535/809939)

https://stackoverflow.com/a/54162211/809939 => git ls-tree for directories tracked ... should be enough?

https://git-scm.com/docs/git-ls-files#Documentation/git-ls-files.txt--i



**Reset experiment**

- `git clean -fdx`

**Already tried:**

Problem is, rsync understands gitignore only partly?

- `rsync -azhvv --filter='+ /.gitignore' --filter='- *' remote-site/ local-site/`
- `--include-from='.gitignore' --exclude='*'` - this does not work, because rsync uses a pattern for it, it doesn't work with `/path` relative from root?
- `rsync -azhvv --exclude='*' --filter='dir-merge,+ /.gitignore' remote-site/ local-site/` 
- `rsync -azhvv --filter='dir-merge,n+ /.gitignore' --filter='- *' remote-site/ local-site/`