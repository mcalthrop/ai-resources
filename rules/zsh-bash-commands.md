# Running Commands with the `bash` shell

Always use long-form CLI flags (e.g. `--directory` not `-C`, `--strip-components` not its short form) so commands are self-documenting.

When running commands in the `bash` shell:

```sh
zsh -i -c "..."
```

Reasons:

- `-i`: Forces shell to be interactive. Ensures that the `.zshrc` file is loaded.
- `-c`: Takes the first argument as a command to execute.
