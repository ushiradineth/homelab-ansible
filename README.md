# 🧪 Homelab Configuration

- This repository consists of code that helps me boot up my 'home server' from scratch with a few simple commands.
- My motivation for this is due to my old laptop failing beyond repair at times which requires me to restart configuration from the beginning.

## 🔧 Stack

- NixOS
- Ansible
- Docker
- Kubernetes

## 🐋 Docker Services

- [x] Portainer (Docker UI)
- [x] Uptime Kuma
- [x] Observability for system and network. (Prometheus, Grafana, Node Exporter)
- [ ] Crafty (Minecraft Server)
- [ ] Homer (Dashboard)
- [ ] Network Drive with Google Drive like UI (Samba + NextCloud or PhotoPrism)
- [ ] Pihole or Adguard (Adblocker and DNS)
- [ ] PFSense or OpenSense (Firewall)
- [ ] VPN (Tailscale or Wireguard)
- [ ] KASM (?)
- [ ] CasaOS (?)

## 🏗️ Kubernetes Services

- [ ] CertManager
- [ ] Traefik
- [ ] Zalando (PostgreSQL Cluster)
- [ ] Knative
- [ ] Observability Stack

## 📂 Ansible Documentation

### 📖 Playbooks

- nixos

  - Purpose: To generate and build the NixOS configuration.
  - Command: `ansible-playbook -i user@host, nixos.yml --ask-become-pass`
  - Note: --ask-become-pass is required as this playbook needs to build and update the OS.

- dotfiles

  - Purpose: To pull and stow dotfiles.
  - Command: `ansible-playbook -i user@host, dotfiles.yml --ask-become-pass`
  - Note: --ask-become-pass is required as this playbook creates the dotfiles for the root user aswell.

- docker

  - Purpose: To create docker configuration and deploy the services.
  - Command: `ansible-playbook -i user@host, docker.yml`
  - Note:
    - This playbook will create a .env file and return early if the .env file is not found on the remote machine, ensure that this is filled as needed.
    - This playbook will not restart services when environment variables change as this does not keep track of them, restart the services manually if the environment variables do change.

### 👷 Roles

- ssh_port

  - Check if the default SSH port is changed to a custom one.
  - Set the SSH Port to the port which the script can connect to successfully.
  - Throw an error if both default and custom SSH ports fails.

- nixos

  - Generate configuration.nix file with the variables.
  - Build and switch to the new configuration if the configuration.nix file is changed.

- dotfiles

  - Clone or Pull the dotfiles repository into the given directory.
  - Stow the files if the dotfiles are changed.

- docker

  - Copy the compose file from the local directory to the remote directory.
  - Deploy the service(s) defined in the compose file.
