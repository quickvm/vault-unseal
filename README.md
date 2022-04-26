# vault-unseal
[Unseal](https://www.vaultproject.io/docs/concepts/seal) your [Hashicorp Vault](https://www.vaultproject.io/) service on when it starts with systemd. This systemd unit and script works best when you install Hashicorp Vault on your workstation or test server using the [official packages](https://www.vaultproject.io/downloads) via your Linux distro's package manager.

**Why was this made?**

Running a local Vault on your Linux workstation can help be useful for side projects, homelabs, and for development work flows. Yes, development mode exists but this gives you a much more persistent Vault experience.

**_!!Warning!!_**
Do not use this automatic unseal in production!

Storing your Vault unseal keys on the same disk as your production Vault service is a _BAD_ idea. Yes the keys file `/etc/vault.d/keys` is owned by root and it is read only and one could argue that if someone had root level access this server and they could do things to intercept unseal requests or other nasty things to get your secrets data. Let's not go there.

Do not use this to unseal your vault cluster if your secret data is important. Use a KMS, another Vault server for a better, more secure [Auto Unseal](https://learn.hashicorp.com/collections/vault/auto-unseal) method or if you have big $$$ you can use a HSM + [Hashicorp Vault Enterprise](https://www.vaultproject.io/docs/enterprise/hsm) instead! Please don't use this for anything important. You have been warned.

## Install

1. Install Hashicorp Vault (https://www.vaultproject.io/downloads)
2. Initialize your Vault (https://www.vaultproject.io/docs/commands/operator/init) and copy the unseal keys into `/etc/vault.d/keys`
  * Don't forget to record your initial root token too!
3. Secure the `/etc/vault.d/keys` file: `chmod 0600 /etc/vault.d/keys $$ chmod root:root /etc/vault.d/keys`
4. Clone down this repo: `git clone git@github.com:quickvm/vault-unseal.git`
5. Enter the repo directory `cd vault-unseal`
6. Run `sudo ./install`

Great job, vault-unseal is now installed! When you start or restart vault.service your Vault will be unsealed automatically.

## Uninstall
If you want to uninstall vault-unseal please manually run these commands as the root user:

```
sudo rm -f /etc/systemd/system/vault.service.d/override.conf
sudo systemctl disable vault-unseal.service
sudo rm -f /usr/local/bin/vault-unseal
sudo rm -f /etc/systemd/system/vault-unseal.service
sudo systemctl daemon-reload
```
## License

MIT License

Copyright (c) 2022 QuickVM

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
