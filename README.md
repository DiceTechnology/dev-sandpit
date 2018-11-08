# dev-sandpit
A handy reset-able development play thing with common tools out of the box


# Getting started

### Dependencies

install vagrant and ansible on your machine with your preferred method (eg, brew,
  direct download etc..)


###### MacOS

```
brew cask install virtualbox
brew cask install vagrant
brew install ansible
```

###### Linux

```
You can work this out yourself smart arse
```


### Dice pre-configuration steps to enable automated ansible deploys

You will need:

(1) decide on vm user account name

(2) Assigned aws key & aws_secret

(3) ansible vault encryption keys

(4) ssh keys (generated with `ssh-keygen` and added to github)

(5) Video-transcoding.pem private ssh key for aws hosts

##### Configure vagrant ansible vars

edit `/dev-sandpit/Vagrantfile` and check the ansible 'extra_vars' variables are correct.

You will need to either set the environment variables on the host system, or manually
change the values:

```
ansible.extra_vars = {
  dice_user: ENV['LOGNAME'], # works on mac
  aws_key: ENV['AWS_ACCESS_KEY_ID']
  aws_secret: ENV['AWS_SECRET_ACCESS_KEY']
  ansible_vault_key: ENV['ANSIBLE_VAULT_KEY']
  dice_env: "local_dev"
}
```


##### (1) Your username

set your username in Vagrant file:

if `dice_user: ENV['LOGNAME']` then `export LOGNAME=jonsnow`

or

`dice_user: ned_stark`



##### (2) AWS keys

You will need dice aws key/secret to enable ansible deploy permissions, either export shell
variables, or manually edit the Vagrantfile as above.

```
export AWS_ACCESS_KEY_ID=<your key>
export AWS_SECRET_ACCESS_KEY<your secret>
```

##### (3) Ansible vault key

make sure you have `~/.ansible.cfg` file on your machine with these contents (pointing to a real ansible vault key):
```
[defaults]
vault_password_file = /path/to/your/local/ansible.key
```
When provisioning the VM, scripts will pick up this config and copy accross the vault key to the VM

##### (4) Your SSH keys

Your user ssh keys in `~/.ssh` should be configured on github to enbale access to dice repositories

##### (4) Aws ssh key

get copy of `Video-transcoding.pem` from a friendly team member and put it in: `/home/<your user>/.ssh/``
on your current machine (ie: not the vm)



### Provisioning the Sandpit Server

`make clean build`


### Logging in the Sandpit server

to login as vagrant admin user:

`vagrant ssh`

or to log in as you:

`ssh -p 2222 <user>@localhost`

### re-provision VM

After ansible config changes or creating new ansible roles:

`make reprovision`