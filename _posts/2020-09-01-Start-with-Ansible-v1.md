---
layout: post
comments: true
title : Start with Ansible v1
categories: [ ansible, provisioning, automation ]
author: John Stilia
---

<span class="img-container">
    <img src="/assets/Start-with-Ansible-01.png" >
</span>


There must be times that you are unhappy and having enough of doing the same task on multiple machines.<br>
There must be times that you are upset of the same task having multiple steps and complex playbooks to follow.<br>
Finding your self in the Abyss of software installation and configurations that have dependencies on other steps not documented?

There is a solution, use Ansible.

Ansible is a simple to use Configuration Management tool that is based on yaml ( or json ) playbooks. Ansible is using normal SSH access to execute the pre-configured steps and it can follow dependencies and playbooks to your liking.

Using Ansible at first might be complicated but rest assured after few hours you will be writing your own playbooks abd complex configurations.

Lets dive in.

---

To begging lets install Ansible on our operating system, detailed instructions can be found [here](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).
My chosen way to install Ansible is by using  **python**.

### Install PIP

```
$ curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
$ python get-pip.py --user
```
### Install anible on user space
```
$ pip install --user ansible
```

---

Ok, and now what?

Lets do some simple tasks with Ansible