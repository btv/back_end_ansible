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

### Deploying the Frontend
To deploy the Angular frontend to the website, one can use a command similar to the one below. Please change the `frontend_directory` variable to the one appropriate for your system.

```
ansible-playbook -i inventory playbook.yml -e frontend_directory=/path/to/the/local/frontend/repo --tags frontend_docs
```
the output would look similar to this:

```
PLAY [all] ************************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************************
ok: [35.239.185.148]

TASK [ded_frontend_docs : generating new angular files] ***************************************************************************************
ok: [35.239.185.148 -> localhost]

TASK [ded_frontend_docs : deploy angular files] ***********************************************************************************************
ok: [35.239.185.148]

PLAY RECAP ************************************************************************************************************************************
35.239.185.148             : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Deploying the Backend
The rust backend runs its own HTTP server. Because of this, the system needs a structure in place to make sure that it can run. Please change the `backend_directory` variable to the one appropriate for your system.

```
ansible-playbook -i inventory playbook.yml -e backend_directory=/path/to/the/local/frontend/repo --tags backend
```

the output would look similar to:

```
PLAY [all] ************************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************************
ok: [35.239.185.148]

TASK [ded_backend : make sure ded_backend directory exists] ***********************************************************************************
ok: [35.239.185.148]

TASK [ded_backend : copy ded_backend systemd file] ********************************************************************************************
ok: [35.239.185.148]

TASK [ded_backend : make sure its running] ****************************************************************************************************
ok: [35.239.185.148]

TASK [ded_backend : generate new production executable] ***************************************************************************************
ok: [35.239.185.148 -> localhost]

TASK [ded_backend : copy all backend files] ***************************************************************************************************
changed: [35.239.185.148]

PLAY RECAP ************************************************************************************************************************************
35.239.185.148             : ok=6    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
...
```
**NOTE**: The command to generate the documentation will only be called if the directory `$PATH_TO_BACKEND/target/release` does not already exist. If you use this command the documentation isn't changing on the server, please make sure to remove that directory and try again.


### Deploying the Backend Documentation
The nginx server that serves the Angular app to our users also hosts the backend documenation. Which can be viewed [here](http://ded.scrollingtext.org:8888/DED_backend/index.html). To deploy these html files, one must use this command:

```
ansible-playbook -i inventory playbook.yml -e backend_directory=/path/to/the/local/frontend/repo --tags backend_docs
```

which will produce an output similar to:

```
PLAY [all] ************************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************************
ok: [35.239.185.148]

TASK [ded_backend_docs : make sure ded_backend docs directory exists] *************************************************************************
ok: [35.239.185.148]

TASK [ded_backend_docs : generating new docs] *************************************************************************************************
changed: [35.239.185.148 -> localhost]

TASK [ded_backend_docs : copy all backend doc file] *******************************************************************************************
changed: [35.239.185.148]

PLAY RECAP ************************************************************************************************************************************
35.239.185.148             : ok=4    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

**NOTE**: The command to generate the documentation will only be called if the directory `$PATH_TO_BACKEND/target/doc` does not already exist. If you use this command the documentation isn't changing on the server, please make sure to remove that directory and try again.