# SiteGround SSH

Use this workflow when a project needs SSH/SFTP access to SiteGround, especially when Cursor or the terminal cannot connect cleanly.

## Goal

Get a reliable SSH connection to the SiteGround site without guessing, changing unrelated SSH config, or running deploy/sync commands before access is verified.

## Steps

1. Get the current SSH credentials from SiteGround:
   - `Site Tools` → `Devs` → `SSH Keys Manager`
   - Capture `Hostname`, `Username`, and `Port`.

2. Use or create an SSH key:
   - Prefer an existing project-specific key if one is already known.
   - If creating a new key, use:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/siteground_project_name -C "siteground-project-name"
```

3. Import the public key in SiteGround:

```bash
cat ~/.ssh/siteground_project_name.pub
```

In SiteGround, import it through:

```txt
Site Tools → Devs → SSH Keys Manager → Add New → Import
```

Do not try to paste a public key into an existing/generated key. Existing generated keys have fixed public/private pairs. If the user only sees actions like `SSH Credentials`, `Manage IP Access`, `Private Key`, `Public Key`, `Change Key Name`, and `Delete`, they are looking at an existing key row, not the import form. Look above it for `Add New`, then switch from `Generate` to `Import`.

If SiteGround truly cannot import a local public key, use the SiteGround-generated key as a fallback:

- Generate/create the key in SiteGround.
- Open `Actions` → `Private Key`.
- Save the private key locally, for example:

```bash
nano ~/.ssh/siteground_project_name
chmod 600 ~/.ssh/siteground_project_name
```

- If the key has a passphrase, have the user load it privately:

```bash
ssh-add ~/.ssh/siteground_project_name
```

Do not ask the user to paste a private key or key passphrase into chat.

4. Check DNS before assuming auth is broken:

```bash
dig +short ssh.example.sg-host.com
dig +short ssh.example.sg-host.com @1.1.1.1
```

If normal DNS fails but public DNS works, use the resolved IP with `HostName`.

5. Test SSH directly:

```bash
ssh -i ~/.ssh/siteground_project_name \
  -p 18765 \
  -o HostName=<resolved-ip-if-needed> \
  <username>@ssh.example.sg-host.com
```

Keep the original SiteGround hostname in the user/host portion, but override `HostName` with the resolved IP when local DNS is stale.

6. Optional: add an SSH config alias only after direct SSH works:

```sshconfig
Host siteground-project
  HostName <resolved-ip-or-hostname>
  HostKeyAlias ssh.example.sg-host.com
  User <username>
  Port 18765
  IdentityFile ~/.ssh/siteground_project_name
  IdentitiesOnly yes
```

Then test:

```bash
ssh siteground-project
```

7. Find the WordPress root:

```bash
pwd
find ~/www -maxdepth 4 -name wp-config.php -print
```

Typical SiteGround path:

```txt
/home/customer/www/<site-domain>/public_html
```

8. Verify WordPress/PHP from the site root:

```bash
cd /home/customer/www/<site-domain>/public_html
php -r 'echo "before\n"; require "wp-load.php"; echo "after\n";'
```

If multiple roots are found, identify production and staging paths separately. Typical examples:

```txt
/home/<user>/www/example.org/public_html
/home/<user>/www/staging6.example.org/public_html
```

Stop after SSH access and the WordPress root path are confirmed unless the user explicitly asks to deploy, sync, edit, or inspect further.

## Safety

- Do not run deploy, sync, rsync, import, or database commands until SSH and the WordPress root are confirmed.
- Before any remote database/content sync, make a remote backup first.
- If SSH fails, distinguish DNS failure from authentication failure:
  - DNS failure: hostname does not resolve.
  - Auth failure: host resolves, connection opens, key/password is rejected.
- Do not edit global SSH config unless needed. Prefer a project-specific host alias.
