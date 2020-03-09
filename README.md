# back end ansible

This repo is for using ansible to create and maintain state for our Vitual Machine running in GCP.

To use this repo, you'll need to make sure that [ansible](https://www.ansible.com/) is installed for your machine:

* For OSX users, just use homebrew:

```
brew install ansible
```

* For any linux users, look into installing ansible from your distro's package manager.


Once ansible is installed the other thing to do is make sure that your SSH public key is on the VM instance. If you don't have this yet, reach out to [Bryce](https://github.com/btv) to help you get it on there.

Lastly, once these two things are in place, follow these commands:

```
git clone git@github.com:btv/back_end_ansible.git
cd back_end_ansible
ansible-playbook -i inventory playbook.yml
```

If its working, you'll see output similar to this:

```
PLAY [all] *************************************************************************

TASK [Gathering Facts] *************************************************************************
ok: [35.239.185.148]

TASK [nginx : make sure nginx is installed] *************************************************************************
ok: [35.239.185.148]
...
```