# Ansible Content Signing Demo


```shell
## Upload test collection
ansible-galaxy collection publish ~/Downloads/ansible-test_collection-1.0.0.tar.gz -c

## Upload community lab collection
ansible-galaxy collection publish ~/Downloads/community-lab_collection-1.0.0.tar.gz -c

```

Import GPG Keys to Keyring

```shell
gpg --import --no-default-keyring --keyring ~/keyring.kbx galaxy_signing_service.asc
```

Install Collection without checking

```shell
$ ansible-galaxy collection install ansible.test_collection -c

Starting galaxy collection install process
Process install dependency map
Starting collection install process
Downloading https://10.5.0.244/api/galaxy/v3/plugin/ansible/content/published/collections/artifacts/ansible-test_collection-1.0.0.tar.gz to /home/rhel/.ansible/tmp/ansible-local-17789l9d5n1u0/tmpd2fw5xl8/ansible-test_collection-1.0.0-0npd3h3i
[WARNING]: The GnuPG keyring used for collection signature verification was not configured but signatures were provided by the Galaxy server to verify
authenticity. Configure a keyring for ansible-galaxy to use or disable signature verification. Skipping signature verification.
Installing 'ansible.test_collection:1.0.0' to '/home/rhel/.ansible/collections/ansible_collections/ansible/test_collection'
ansible.test_collection:1.0.0 was installed successfully
```

Install Collection without checking

```shell
$ nsible-galaxy collection install community.lab_collection --keyring /home/rhel/keyring.kbx -c -vvvv
...<removed for brevity>...
Installing 'community.lab_collection:1.0.0' to '/home/rhel/.ansible/collections/ansible_collections/community/lab_collection'
Running command '['gpg', '--status-fd=5', '--verify', '--batch', '--no-tty', '--no-default-keyring', '--keyring=/home/rhel/keyring.kbx', '-', '/home/rhel/.ansible/collections/ansible_collections/community/lab_collection/MANIFEST.json']'
stdout: 
[GNUPG:] NEWSIG
[GNUPG:] KEY_CONSIDERED 0E450BAA796C33F8A6FBA69BC97AC9643F917EBB 0
[GNUPG:] SIG_ID aGHlooh6a0hYjI+nOMutAmPEcsg 2023-05-24 1684935652
[GNUPG:] KEY_CONSIDERED 0E450BAA796C33F8A6FBA69BC97AC9643F917EBB 0
[GNUPG:] GOODSIG C97AC9643F917EBB Instruqt Redhat (with no passphrase) <instruqt@ansible.com>
[GNUPG:] VALIDSIG 0E450BAA796C33F8A6FBA69BC97AC9643F917EBB 2023-05-24 1684935652 0 4 0 1 8 00 0E450BAA796C33F8A6FBA69BC97AC9643F917EBB
[GNUPG:] KEY_CONSIDERED 0E450BAA796C33F8A6FBA69BC97AC9643F917EBB 0
[GNUPG:] TRUST_UNDEFINED 0 pgp
[GNUPG:] VERIFICATION_COMPLIANCE_MODE 23

stderr: 
gpg: Signature made Wed 24 May 2023 01:40:52 PM UTC
gpg:                using RSA key 0E450BAA796C33F8A6FBA69BC97AC9643F917EBB
gpg: Good signature from "Instruqt Redhat (with no passphrase) <instruqt@ansible.com>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 0E45 0BAA 796C 33F8 A6FB  A69B C97A C964 3F91 7EBB

(exit code 0)
GnuPG signature verification succeeded for community.lab_collection
community.lab_collection:1.0.0 was installed successfully
```

Verify artifacts

```shell
$ cat ~/.ansible/collections/ansible_collections/community.lab_collection-1.0.0.info/GALAXY.yml
```

Validate content using `ansible-galaxy`

```shell
## modify Content
$ touch ~/.ansible/collections/ansible_collections/community/lab_collection/docs/README.md

## validate
$ ansible-galaxy collection verify community.lab_collection --keyring keyring.kbx -c
Verifying 'community.lab_collection:1.0.0'.
Installed collection found at '/home/rhel/.ansible/collections/ansible_collections/community/lab_collection'
MANIFEST.json hash: a77953a136fe7d70f0d792f698a05f91dab069c8cdad6da04459e0532c6c1ba9
Collection community.lab_collection contains modified content in the following files:
    docs/README.md
```