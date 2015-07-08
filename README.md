# vagrant-vm-for-dev-with-docker

Vagrant Virtual Machine for development - when you don't want to use separate VMs for every project you work on, but instead a single VM and (hopefully) separate Docker containers for your separate projects.

Supports VirtualBox and Parallels vagrant VM providers at the moment,
but it's easy to extend it (in the `Vagrantfile`) with a new one.


## Setup

Required tools:

* `vagrant (brew cask install vagrant)`
* `ansible (brew install ansible)`
* `virtualbox (brew cask install virtualbox) or parallels`

Set these Environment Variables (best to do so in your `bash` / `fish` / etc shell profile,
for `bash` you should add these to your `~/.bash_profile`):

* `echo >> ~/.bash_profile`
* `echo "export MY_DEV_PATH=path/to/your/development/folder >> ~/.bash_profile`"
* `echo "export VAGRANT_DEFAULT_PROVIDER=[virtualbox/parallels] >> ~/.bash_profile`"
* `source ~/.bash_profile`

Or export these Environment Variables in session wich use vagrant-vm-for-dev-with-docker

* `export MY_DEV_PATH=path/to/your/development/folder`
* `export VAGRANT_DEFAULT_PROVIDER=[virtualbox/parallels]`

Start this `vagrant` VM: `vagrant up --provision`

If you change any provisioning configuration (ex: the `vagrantfiles_playbook.yml` file)
you should always run vagrant up with `--provision` to do the provisioning again.
If the VM is already running you can reload it and redo
provisioning with: `vagrant reload --provision`.


## Installed

Setup installs the following tools on the VM:

* `docker`
* `docker-compose`
* `git`
* `curl`

The full list of tools can be found in `vagrant_files/playbook.yml`
except `docker` which is installed with vagrant's built-in
docker install command (`config.vm.provision "docker"`).


## Usage

Once the setup is complete you can:

* start the VM with: `vagrant up`
* log (ssh) into the VM: `vagrant ssh`
* stop the VM: quit from the VM with `exit` and then `vagrant halt`

To use `docker-compose` you have to use `sudo` or switch
to super user with `sudo su`.
