# vagrant-hashistack

![CI/CD](https://github.com/fredrikhgrelland/vagrant-hashistack/workflows/CI/CD/badge.svg)
![version](https://img.shields.io/badge/dynamic/json?label=latest%20version&query=%24.current_version.version&url=https%3A%2F%2Fapp.vagrantup.com%2Fapi%2Fv1%2Fbox%2Ffredrikhgrelland%2Fhashistack)
![updated](https://img.shields.io/badge/dynamic/json?label=updated&query=%24.current_version.updated_at&url=https%3A%2F%2Fapp.vagrantup.com%2Fapi%2Fv1%2Fbox%2Ffredrikhgrelland%2Fhashistack)

This vagrant box aims to make it dead simple to start a hashistack emulating how services will deploy to production.

This repository will release a new [template](template/README.md) into [fredrikhgrelland/vagrant-hashistack-template](https://github.com/fredrikhgrelland/vagrant-hashistack-template) on every release.

---

<p align="center">
   <a href="https://app.vagrantup.com/fredrikhgrelland/boxes/hashistack" alt="Download og Vagrant Cloud">
        <img src="https://img.shields.io/badge/Download%20on-Vagrant%20Cloud-orange?style=for-the-badge&logo=vagrant" /></a>
   <a href="https://github.com/fredrikhgrelland/vagrant-hashistack-template" alt="Clone Template">
     <img src="https://img.shields.io/badge/Github-Clone%20template-blue?style=for-the-badge&logo=github" /></a>
</p>

---

> 🚧 - current vagrant box runs consul, nomad and vault in `dev` (development) mode.
- [consul development mode](https://learn.hashicorp.com/consul/getting-started/agent)
- [nomad development mode](https://learn.hashicorp.com/nomad/getting-started/running)
- [vault development mode](https://www.vaultproject.io/docs/concepts/dev-server)
---
## Prerequisites

You will need to have pre-installed:

- [Make](https://man7.org/linux/man-pages/man1/make.1.html)

The rest of the prerequisites are system-dependent:

### Linux
- Virtualisation must be enabled. [Error if it is not.](https://github.com/fredrikhgrelland/vagrant-hashistack/issues/136)
- Packages `gpg` and `apt` must be installed.

### MacOS
- Virtualisation must be enabled. [This is on by default on MacOS.](https://support.apple.com/en-us/HT203296)
- [Homebrew](https://brew.sh/) must be installed.

## Build & Test

`make install` (ubuntu 18.04 or macos) will download and install all prerequisites (virtualbox, vagrant, packer)

`make build` will build a vagrant box based on hashicorp/bionic64. The packaged box will be locally available at ´packer/output-hashistack/package.box´

`make test` run tests by starting the [countdash](https://www.nomadproject.io/docs/integrations/consul-connect/) consul-connect example. If ´packer/output-hashistack/package.box´ does not exist, it will run ´make build´

## Usage

This repo will build a base-box for different projects to extend on. The base box contains components and a setup that makes it ideal for working with the hashistack.



The default box will start Nomad, Vault and Consul, bound on loopback and advertising on the ip `10.0.3.10`, which should be available on your local machine.
Portforwarding for nomad on port `4646` should bind to `127.0.0.1` and should allow you to use the nomad binary to post jobs directly. Consul and Vault has also been portforwarded, and are also available on `127.0.0.1` on port `8500` and `8200` respectively.
- Nomad ui is available on [http://10.0.3.10:4646](http://10.0.3.10:4646) and all links to services should work.
- Consul ui is available on [http://10.0.3.10:8500](http://10.0.3.10:8500)
- Vault ui is available on [http://10.0.3.10:8200](http://10.0.3.10:8200)

### Starting a plain default box
To get a running VM using the lastest release of this box run `vagrant init fredrikhgrelland/hashistack` then `vagrant up`. The first command will add a file called `Vagrantfile` to your directory, and `vagrant up` will start a box based on the specifications of that file.

### Starting a new project based on the hashistack
To see a full example of how to start a new project based on this box go to [template-repo](https://github.com/fredrikhgrelland/vagrant-hashistack-template).

### Default master tokens

The master token for `Consul` and `Vault` is `master`.

### If you are behind a transparent proxy

If you for any reason find yourself behind a transparent proxy you need to set the environment variables `SSL_CERT_FILE` and `CURL_CA_BUNDLE`. You have three options:
- Prefix `vagrant up`; `SSL_CERT_FILE=<path/to/ca-certificates-file> CURL_CA_BUNDLE=<path/to/ca-certificates-file> vagrant up`
- Set the environment variables in your current session by running `export SSL_CERT_FILE=<path/to/ca-certificates-file>` and `export CURL_CA_BUNDLE=<path/to/ca-certificates-file>` in the terminal. Eg:`export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt CURL_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt`
- Set the environment variables permanently by adding the export commands above to your `~/.bashrc` or equivalent.

## Why does this exist?

We needed a Vagrant box with the complete hashistack to use for demo, development and testing.
In order to build cloud native, security minded and dependable services, there exists a killer combination;
- Containers - (Docker)
- Simple&Powerful Orchestrator - (Nomad)
- Service-mesh mTLS - (Consul connect)
- Secrets management - (Vault)

### Hashistack

- Consul
- Nomad
- Vault
- Terraform
- Docker CE

#### - with a side-play of

- Ansible (installed)
- Packer
- consul-template

## Contribute

[See here](docs/CONTRIBUTING.md)
