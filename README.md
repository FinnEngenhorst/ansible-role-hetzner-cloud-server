# Hetzner Cloud Server Setup Role

This Ansible role automates the creation of Hetzner Cloud servers, configuring them with a cloud-config template, and can assign Labels. 
It uses the Hetzner Cloud API to create the servers, and it can be customized to fit different cloud configurations.

## Requirements

- **Ansible 2.9 or later**: This role requires Ansible to be installed on the control machine.
- **Hetzner Cloud API Token (`HCLOUD_TOKEN`)**: An active Hetzner Cloud API token is needed to authenticate and create servers.
- **SSH Public Key (`common_ssh_public_key`)**: A valid SSH public key to be used for server access.

## Role Variables

- `hetzner_api_token`: The Hetzner Cloud API token used to authenticate with the Hetzner API. This should be set as an environment variable `HCLOUD_TOKEN` or can be manually defined.
  
- `hetzner_servers`: A list of dictionaries, each containing the configuration for a Hetzner server. Each server configuration must include the following:
  - `name`: The name of the server.
  - `type`: The type of server (e.g., `cx11`, `cx21`).
  - `datacenter`: The datacenter where the server will be created (e.g., `fsn1-dc14`, `nbg1-dc3`).
  - `image`: The server image to use (e.g., `ubuntu-20.04`).
  - `labels`: (Optional) Labels to apply to the server, useful for categorization (e.g., `env: production`, `role: web`).

- `common_user_name`: The username for a common user to be created on all servers (default: `"newuser"`).
- `common_ssh_public_key`: The SSH public key to be added to the `common_user_name`'s `authorized_keys` for SSH access.

### Example:

```yaml
---
- name: Create and configure Hetzner Cloud servers
  hosts: localhost
  gather_facts: no
  vars:
    hetzner_api_token: "{{ lookup('env', 'HCLOUD_TOKEN') }}"  # Set your Hetzner API token
    hetzner_servers:
      - name: server-1
        type: cx21
        datacenter: fsn1-dc14
        image: ubuntu-20.04
        labels:
          env: production
          role: web
      - name: server-2
        type: cx11
        datacenter: nbg1-dc3
        image: ubuntu-20.04
        labels:
          env: staging
          role: db
    common_user_name: "newuser"  # New Admin User
    common_ssh_public_key: "ssh-rsa AAAAB3...your-public-key... comment"  # Your SSH public key for the Admin user
```

## License

This project is licensed under the MIT License

