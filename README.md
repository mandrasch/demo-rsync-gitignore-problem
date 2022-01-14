
**Goal**

- Rsync everything from `remote-site/` to `local-site/`-folder, 
    - EXCEPT for git tracked folder and files (e.g. `wp-content/themes/twentytwentyone-child`). 
    - Should be based on `local-site/.gitignore` (basically we need to opposite of it)

Challenge is that rsync understands gitignore only partly (?) and can't work with the following `.gitignore` pattern with slashes at the beginning:
```
# Ignore all ... 
# (via relative from docroot, not via glob)
/*

# ... but track specific files / folders: 
# (see https://stackoverflow.com/a/29012085/809939)

!.gitignore

# track child theme
!/wp-content
/wp-content/*
!/wp-content/themes
/wp-content/themes/*
!/wp-content/themes/twentytwentyone-child
```

Therefore `--include-from='.gitignore' --exclude='*'` - does not work, because rsync excepts patterns for it, not paths with `/...`.

**Current try: git ls-files and ls-tree**

Does not work as well:

```bash
cd demo-rsync-gitignore-problem
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



- `rsync -azhvv --filter='+ /.gitignore' --filter='- *' remote-site/ local-site/`
- `--include-from='.gitignore' --exclude='*'` - this does not work, because rsync uses a pattern for it, it doesn't work with `/path` relative from root?
- `rsync -azhvv --exclude='*' --filter='dir-merge,+ /.gitignore' remote-site/ local-site/` 
- `rsync -azhvv --filter='dir-merge,n+ /.gitignore' --filter='- *' remote-site/ local-site/`