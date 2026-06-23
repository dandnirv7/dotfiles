# dotfiles

Chezmoi-managed dotfiles untuk setup harian. Source directory:
`~/.local/share/chezmoi`. Remote: `https://github.com/dandnirv7/dotfiles`.

## Workflow harian (manual, backup-only)

Pola pikir: `~` adalah source of truth, repo ini hanya mirror/backup.

1. Edit file di `~` seperti biasa (mis. ubah keybind Hyprland, tweak nvim).
2. Tambah ke source: `cza ~/.config/hypr/hyprland.conf` (atau `cza --recursive ~/.config/nvim`).
3. Commit + push: `czp "hyprland: bind SUPER+RETURN for terminal"`.

`czp` tanpa argumen → message default `"update dotfiles"`.

## Restore / Onboard mesin baru

```bash
# Install chezmoi
sh -c "$(curl -fsLS get.chezmoi.io)"

# Clone & apply (hanya sekali di mesin baru)
chezmoi init --apply https://github.com/dandnirv7/dotfiles.git
```

Untuk restore ke mesin **yang sama** (mis. setelah reinstall):
```bash
# Clone repo, lalu init source-only (no apply)
git clone https://github.com/dandnirv7/dotfiles.git ~/.local/share/chezmoi
# Review: czd (diff), czs (status), lalu apply manual:
cz apply
```

## Perhatian

- `chezmoi apply` **tidak** dijalankan di workflow harian. Hanya dipakai saat restore.
- Repo di-trim: tema Hyde, theme Spotify, cache browser yazi, dictionaries nvim tidak di-include (file besar/generasi otomatis).
- Secrets (`.aws`, `.gnupg`, SSH private keys, `.git-credentials`) **tidak pernah** di-add. Pakai `chezmoi add --encrypt` kalau perlu simpan secret.

## Multi-host (rencana)

Saat ini single host (`cachyos`). Untuk multi-host nanti:
- Branch per host: `git checkout -b laptop`, `git checkout -b server` di source.
- Serap perubahan hulu: `cz merge` atau `git merge main`.
- Push balik: `git push origin laptop`.

Template `chezmoi.toml.tmpl` sudah punya `[data]` block siap pakai variabel
seperti `{{ .chezmoi.hostname }}` saat dibutuhkan.
