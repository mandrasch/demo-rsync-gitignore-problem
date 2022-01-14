
**Goal**

- Rsync everything from remote-site/ to local-site/-folder, 
    - except for git tracked folder and files (wp-content/themes/twenttwentyone-child). 
    - Should be based on local-site/.gitignore (reverse / opposite).

```bash
cd demo-rsync-gitignore-problem
rsync -azhvv --filter=':- .gitignore' remote-site/ local-site/
```

**Already tried:**

- `--include-from='.gitignore' --exclude='*'` - this does not work, because rsync uses a pattern for it, it doesn't work with `/path` relative from root?

**Reset:**

- `git clean -fdx`