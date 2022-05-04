### Yubikey

Verify Key: https://www.yubico.com/genuine/

[![Codacy Badge](https://app.codacy.com/project/badge/Grade/96dc969b9b364ae48552a90e9d12828b)](https://www.codacy.com/gh/mikesupertrampster-corp/yubikey/dashboard?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=mikesupertrampster-corp/yubikey&amp;utm_campaign=Badge_Grade)

###### Enable Smart Card Daemon

```nix
  services = {
    xserver = {

    # Yubikey
    pcscd.enable = true;
    udev = {
      packages = [ pkgs.yubikey-personalization ];
      extraRules = ''
        KERNEL=="hidraw*", SUBSYSTEM=="hidraw", MODE="0660", GROUP="plugdev", ATTRS{idVendor}=="1050"
      '';
    };
  };
```

###### Enable GPG Agent

```nix
  environment.shellInit = ''
    export GPG_TTY="$(tty)"
    gpg-connect-agent /bye
    export SSH_AUTH_SOCK="/run/user/$UID/gnupg/S.gpg-agent.ssh"
  '';
```

or BASH:

```bash
export GPG_TTY=/dev/pts/0
gpg-connect-agent updatestartuptty /bye
unset SSH_AGENT_PID
export SSH_AUTH_SOCK=/run/user/1000/gnupg/S.gpg-agent.ssh
```

#### Configuration

```bash
gpg --card-status
gpg --edit-card
```

Update PINs:
```bash
admin, then passwd
  # option 3
  # option 1
```

Generate GPG key:

```bash
gpg --card-edit
admin, then generate
(n) backup
(0) expiry
```

Display Public RSA Key:
```bash
ssh-add -L
```
```bash
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDUlbo9Lqkb7/Mf1AH2CSYFozWVxB0NjcpJ6P1RN0/tVA1A/ieMtrNjDtKS5pnQp1pewtc/yn3ATrA02QiP/sKq8oVHHL+78Lawpgu11N2yEdUVnsY5Z7SLTge/pZqpObVKKgWezRzDq1Q/uyunjafFu2eMwJfNYEF3s/E81ruXq2bYVfOlIGMeTGBjzhVwxMR/yL+tdYu/cZmGekyiGJWRQEkoHH8yVjhTCt2FIu5xRvrHe7kXNlFbXvY3KjdUiMYSeDOt4up69xTCkgjp0cLuvzMq+gS6GDcqL8D1fE3d/3tojPzJjOM7tzgwHEapGHLQCGneVQzZ6Ik58XvpqjQ/ cardno:000609920713
```

Export PGP Public Key:
```bash
gpg --armor --export <email>
```

Display LONG Signing Key ID:
```bash
gpg --list-secret-keys --keyid-format LONG
```

```bash
-----BEGIN PGP PUBLIC KEY BLOCK-----

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
imzD/bU3rIy+
=iJSJ
-----END PGP PUBLIC KEY BLOCK-----
```
