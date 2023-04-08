# sops

> SOPS: Secrets OPerationS.
> Tool for managing secrets.
> More information: <https://github.com/mozilla/sops>.

- Encrypt a file:

`sops -e {{path/to/myfile.json}} > {{path/to/myfile.enc.json}}`

- Decrypt a file to the standard output:

`sops -d {{path/to/myfile.enc.json}}`

- Rotate data keys for a sops file:

`sops -r {{path/to/myfile.enc.yaml}}`

- Change the extension of the file once encrypted:

`sops -d --input-type json {{path/to/myfile.enc.json}}`

- Extract keys by naming them, and array elements by numbering them:

`sops -d --extract '["an_array"][1]' {{path/to/myfile.enc.json}}`

- Show the difference between two sops files:

`diff <(sops -d {{path/to/secret1.enc.yaml}}) <(sops -d {{path/to/secret2.enc.yaml}})`


# Custom  ..........................................................................................
- use several keys simultanously for high avail: pgp + kms
- .env, yaml, json
```bash
gpg --fingerprint sysid@gmx.de
echo 7F3F C64B 410A 7E43 D3EB  52E0 FBAF 64AE 3738 9C69 | tr -d ' '
sops -pgp 7F3FC64B410A7E43D3EB52E0FBAF64AE37389C69 pgpfile.yaml
```
## config
[Use yaml config for multiple keys](https://github.com/mozilla/sops#using-sops-yaml-conf-to-select-kms-pgp-for-new-files)

## age vs PGP:

Simplicity:
age is designed to be simple and easy to use, with a minimalistic interface and a focus on basic encryption and decryption tasks.
PGP, on the other hand, is a more complex system with a wide array of options, which can be overwhelming and prone to user error.

Modern cryptography:
age is built on modern cryptographic primitives like X25519, ChaCha20-Poly1305, and Argon2.
PGP relies on older cryptographic standards like RSA, which may be more susceptible to attacks as time goes on and computing power increases.

Key management:
age uses a simple public key model, making it easier to manage and exchange keys.
PGP uses a more complex web of trust model that requires users to understand and manage key signing, which can be difficult for non-experts.

File format:
age uses a custom file format that is more resistant to tampering and easier to parse than the ASCII-armored format used by PGP.

Speed:
age is generally faster than PGP due to its streamlined design and the use of efficient modern cryptographic algorithms.

Codebase size:
age has a smaller codebase compared to PGP, which makes it easier to audit and maintain.


## Resources
[GitHub - mozilla/sops: Simple and flexible tool for managing secrets](https://github.com/mozilla/sops)
https://frederic-hemberger.de/notes/kubernetes/manage-secrets-with-sops/
