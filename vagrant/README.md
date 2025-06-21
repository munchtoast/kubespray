# Vagrant Post-Ansible provisioning

## Purpose

Current Kubespray project only features Vagrant to provision test cluster but no additional operations post provisioning. This directory features additional tasks that Vagrant runs after initial cluster provisioning. These files are designed to be modified depending on user needs.

## Quick Start

By default, a `vagrant up` will automatically execute these post provisioning tasks.

To execute the Ansible separately:

```sh
vagrant provision --provision-with "extend"
```

## Usage

The `Vagrantfile` located at the root of the project features a variable `$extend_ansible` which is a dictionary containing: `playbook` and `vars`. By default, this is automatically pointing to `vagrant/main.yml` and `vagrant/default.yml`.

Additional playbooks can be specified here along with their correlated vars (if applicable) and Vagrant will run procedurally.

To invoke additional tasks while using the same `vagrant/main.yml`, add the task in the `subtask` directory, and vars (if applicable) in the `subtask_vars` directory. Then, update the `input_tasks_run_order` in the `vagrant/default.yml`. The `vagrant/main.yml` playbook will automatically detect the path to these subtasks and execute them procedurally. Variables will be referenced as `extends` within the task (i.e. `extends.test_variable`).

Additionally, each task can utilize multiple vars specified with `input_tasks_run_order` inside the `vagrant/default.yml`.
