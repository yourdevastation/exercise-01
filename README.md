# PostgreSQL Ansible Provisioning with Vagrant

This is a simple example of how to provision a PostgreSQL database using Ansible and Vagrant.

## Requirements

Make sure you have the following installed:

- [Vagrant](https://www.vagrantup.com/downloads)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- [Git](https://git-scm.com/downloads)
- Optional: WSL2 (Windows Subsystem for Linux) if you are on Windows.

## Project Structure

```text
.
├── ansible.cfg                     # Ansible configuration file
├── environments
│   └── local
│       ├── group_vars
│       │   ├── all.yml
│       │   └── postgresql
│       │       ├── postgresql.yml  # Defeault PostgreSQL variables
│       │       └── vault.yml       # Encrypted credentials
│       ├── hosts # Inventory file
│       ├── host_vars
│       └── play_vars
├── LICENSE
├── molecule
│   ├── resources
│   └── scenario_name
├── playbooks
│   └── postgres.yml                # PostgreSQL playbook
├── README.md # This file
├── requirements.txt
├── requirements.yml
├── roles
│   ├── postgres_configure          # Role to configure PostgreSQL
│   │   ├── handlers
│   │   │   └── main.yml
│   │   ├── tasks
│   │   │   └── main.yml
│   │   └── templates
│   │       ├── pg_hba.conf.j2
│   │       └── postgresql.conf.j2
│   └── postgres_install            # Role to install PostgreSQL
│       └── tasks
│           └── main.yml
├── site.yml                        # Main playbook including `postgres.yml'
└── Vagrantfile                     # Vagrant configuration file
```

## Usage

### Clone the repository:

```bash
git clone https://github.com/yourdevastation/exercise-01.git
cd exercise-01
git checkout dev
```

### Ansible Vault

To provide a basic level of security, PostgreSQL credentials are stored in an Ansible Vault file 'environments/local/grop_vars/postgresql/vault.yml' and encrypted with a password "vaultpass" usinng the command:

```bash
ansible-vault encrypt environments/local/group_vars/postgresql/vault.yml
```

To create the vault password file, create a file named `.vault_password.txt` in the root directory of the project and add the password "vaultpass" to it.

```bash
echo "vaultpass" > .vault_password.txt
```

You can change the default vault password file in ansible.cfg under:

```ini
[defaults]
vault_password_file = .vault_password.txt
```

Alternatively, you can decrypt file manually (not recommended for production):

```bash
ansible-vault decrypt environments/local/group_vars/postgresql/vault.yml
```

Never commit `.vault_password.txt` to your repository. It's excluded via `.gitignore`.

### Running the VM

To start and provision the PostgreSQL VM:

```bash
vagrant up
```

Provisioning with Ansible should run automatically. To force it manually:

```bash
vagrant provision
```

You can also run Ansible independently:

```bash
ansible-playbook site.yml
```

### Accessing the PostgreSQL Database

Once the VM is up and running, you can access the PostgreSQL database using the following command from your host machine:

```bash
psql -h 192.168.56.3 -U postgres
```

You will be prompted for the password, which is stored in the Ansible Vault file. To access it, you can use:

```bash
ansible-vault view environments/local/group_vars/postgresql/vault.yml
```

## License

This project is licensed under the MIT License.
