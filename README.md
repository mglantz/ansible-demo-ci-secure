# ansible-demo-ci
Demonstration of how you can use Molecule testing inside of GitHub actions to ensure your Ansible code is always tested.

## Content
```
├── apache.yml # A playbook we test
├── .github # GitHub actions main directory
│   └── workflows 
│       └── ci.yml # CI pipeline which does Molecule testing
├── LICENSE
├── molecule # Molecule main directory
│   └── default # Test scenario default directory
│       ├── converge.yml # How you get Molecule to run a playbook
│       ├── molecule.yml # Molecule main config
│       └── verify.yml # Unit tests
└── README.md
```

# test status:
[![CI](https://github.com/mglantz/ansible-demo-ci/actions/workflows/ci.yml/badge.svg)](https://github.com/mglantz/ansible-demo-ci/actions/workflows/ci.yml)
