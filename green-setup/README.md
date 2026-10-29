# green-setup — customizzazioni personali sopra WhiteRabbitShell (mtdot)

Backup delle mie modifiche al tema rabbit. NON fa parte di mtdot upstream.
Branch `my-setup` del fork `myfoxx/rabbit-dotfiles`.

## Cosa c'è

| File | Dove va | Cosa fa |
|---|---|---|
| `bin/theme-switch` | `~/.local/bin/` | switch green↔rabbit + **patch idempotenti** (NVIDIA Intel, kb_layout it, remap tasti, night mode, ricerca file nel launcher). Ri-applica tutto dopo ogni `dotup`. |
| `bin/dotup` | `~/.local/bin/` | aggiorna `~/my-dotfiles` (stash→pull→pop→`theme-switch patch`→verifica lua) senza perdere le customizzazioni. |
| `config/launcher-fdignore` | `~/.config/` | blacklist per la ricerca file `/` del launcher (cache, estensioni spazzatura). |
| `config/recoll-index.{service,timer}` | `~/.config/systemd/user/` | reindex recoll automatico ogni ora. Abilitare: `systemctl --user enable --now recoll-index.timer`. |
| `recoll/recoll.conf` | `~/.recoll/` | config recoll: whitelist MIME documenti + skippedNames (no codice/cache). |

## Ricerca file nel launcher (Spotlight-style)
Launcher rabbit (backtick / SUPER+D):
- `/nome` → file per nome (fd, cartelle prima) — vedi anche il PR upstream privatefound/my-dotfiles#1
- `//testo` → full-text nei documenti (recoll)
- `=` calc · `>` esegui · `?` Morpheus (nativi mtdot)

## Ripristino su una macchina nuova
```bash
cp green-setup/bin/* ~/.local/bin/ && chmod +x ~/.local/bin/{theme-switch,dotup}
cp green-setup/config/launcher-fdignore ~/.config/
mkdir -p ~/.recoll && cp green-setup/recoll/recoll.conf ~/.recoll/
cp green-setup/config/recoll-index.* ~/.config/systemd/user/
systemctl --user daemon-reload && systemctl --user enable --now recoll-index.timer
theme-switch patch   # applica tutte le customizzazioni a ~/my-dotfiles
```

## Dipendenze
`fd recoll` (`sudo pacman -S fd recoll`), più `fsearch` per SUPER+Space.
