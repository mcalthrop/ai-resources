# Running Commands in a Shell

When running commands in a shell, use the user's current shell in interactive mode so that the shell's `*rc` file is sourced:

```sh
$SHELL -i -c "..."
```

Reasons:

- `$SHELL`: Uses the user's configured shell (e.g. `zsh`, `bash`).
- `-i`: Forces the shell to be interactive. Ensures that the shell's `*rc` file (e.g. `.zshrc`, `.bashrc`) is loaded.
- `-c`: Takes the first argument as a command to execute.
