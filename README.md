packer-provisioner-ansible
=======

packer-provisioner-ansible is a [Packer](https://packer.io/) plugin that
provisions machines using [Ansible](http://docs.ansible.com/).

Limitations
------

packer-provsioner-ansible does not support SCP to transfer files.

Install
======

Download and install Packer: https://github.com/mitchellh/packer#quick-start

It's recommended to build this plugin using `goop`. Install `goop`:

````Shell
go get github.com/nitrous-io/goop && go build github.com/nitrous-io/goop
````

Then build the packer plugin. From within the plugin source code directory use the commands:

````Shell
    goop install && goop go build
````

Copy the binary `packer-provisioner-ansible` to your packer directory.

Getting Started
======

This is a fully functional template that will provision an image on
DigitalOcean. Replace the mock `api_token` value with your own.

````json
{
	"provisioners": [
		{
			"type": "ansible",
			"playbook_file": "./playbook.yml",
			"extra_arguments": ["--private-key", "./id_packer-ansible", "-v", "-c", "paramiko"],
			"ssh_authorized_key_file": "./id_packer-ansible.pub",
			"ssh_host_key_file": "./packer_host_private_key"
		}
	],

	"builders": [
		{
			"type": "digitalocean",
			"api_token": "6a561151587389c7cf8faa2d83e94150a4202da0e2bad34dd2bf236018ffaeeb",
			"image": "ubuntu-14-04-x64",
			"region": "sfo1"
		},
	]
}
````

Configuration Reference
======

required parameters
------

- `playbook_file` - The playbook file to be run by Ansible.
- `ssh_host_key_file` - The SSH key that will be used to run the SSH server to which Ansible connects.
- `ssh_authorized_key_file` - The SSH public key of the Ansible `ssh_user`.

optional parameters
------

- `local_port` (string) - The port on which ansible-provisioner should first
	attempt to listen for SSH connections. This value is a starting point.
	ansible-provisioner will attempt listen for SSH connections on the first
	available of ten ports, starting at `local_port`. The default value is 2200.
- `sftp_command` (string) - The command to run on the machine to handle the
	SFTP protocol that Ansible will use to transfer files. The command should
	read and write on stdin and stdout, respectively. Defaults to
  `/usr/lib/sftp-server -e`.
