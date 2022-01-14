
**Goal**

- Rsync everything from remote-site/ to local-site/-folder, 
    - except for git tracked folder and files (wp-content/themes/twenttwentyone-child). 
    - Should be based on local-site/.gitignore (reverse / opposite).

**Current try (does not work)**

```bash
cd demo-rsync-gitignore-problem
rsync -azhvv --filter=':+ .gitignore' remote-site/ local-site/
```

(Based on https://stackoverflow.com/a/15373763/809939)

**Already tried:**

- `--include-from='.gitignore' --exclude='*'` - this does not work, because rsync uses a pattern for it, it doesn't work with `/path` relative from root?


**Reset experiment**

- `git clean -fdx`