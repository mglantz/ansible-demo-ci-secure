# ansible-demo-ci-secure
This repository container a demo of how you can use Ansible tools to address common development challenges such as testing and supply chain security.

This repository contains a demonstration of the following:
* How you can use Molecule to perform automated testing of content
* How you can use ansible-sign to automatically sign content which passes your testing
* How Ansible Automation Platform can be configured to only run signed code

## What problems does this address?
* Mitigate the risk that faulty code is pushed to production (Molecule testing done for each pull request)
* Mitigate the damage from a developers version control account getting compromised (If the gates you setup in your pipeline are not passed, the code is not signed)
* Ensure that developers follows development guidelines

## Content
```
├── .ansible-sign # Where ansible-sign puts the checksum list of content sign
│   ├── sha256sum.txt
│   └── sha256sum.txt.sig
├── apache.yml # A playbook we test with Molecule
├── .github # GitHub actions main directory
│   └── workflows 
│       └── ci.yml # CI pipeline which does Molecule testing and signing
├── LICENSE
├── MANIFEST.in # This file tells ansible-sign what to sign and what to ignore.
├── molecule # Molecule main directory
│   └── default # Test scenario default directory
│       ├── converge.yml # How you get Molecule to run a playbook
│       ├── molecule.yml # Molecule main config
│       └── verify.yml # Unit tests
├── README.md
└── secure-org_pubkey.asc # The public GPG key, which you use to validate the signed content
```

## How to run this demo
1. Fork this repository
2. Create a GPG keypair for signing
3. Export your public gpg key and replace secure-org_pubkey.asc with your public key. Export the key with:
```
gpg --list-keys
gpg --armor --export THEID
```
4. Go to the GitHub Settings > Secrets and variables > Actions
5. Click New repository secret and name it GPG_PRIVATE_KEY
6. Export the private key with below command and paste it into the field for GPG_PRIVATE_KEY:
```
gpg --list-keys
gpg --armor --export-secret-key THEID
```
7. Click New repository secret and name it GPG_PASSPHRASE and put the passphrase for importing/exporting your key there.
8. Go to Ansible Automation Platform and create a new Credential (GPG Public Key) and paste in your public gpg key.
9. Create a new project and be sure to set "Content signature validation credential" to GPG Public Key Credentials you created in 8. AND also set "Update on launch" for the project.
10. Create a job template which runs the apache.yml playbook.
11. Run the playbook and enjoy that it's not been tampered with.

# test status:
[![CI](https://github.com/mglantz/ansible-demo-ci/actions/workflows/ci.yml/badge.svg)](https://github.com/mglantz/ansible-demo-ci/actions/workflows/ci.yml)
