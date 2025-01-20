#### Variables ####

export ROOT_DIR ?= $(PWD)
export K8S_ROOT_DIR ?= $(ROOT_DIR)

export ANSIBLE_NAME ?= ansible-k8s
export HOSTS_INI_FILE ?= $(K8S_ROOT_DIR)/hosts.ini

export EXTRA_VARS ?= ""

#### Start Ansible Docker ####
k8s-ansible:
	export ANSIBLE_NAME=$(ANSIBLE_NAME); \
	sh $(K8S_ROOT_DIR)/scripts/ansible ssh-agent bash

#### a. Debugging ####
k8s-debug:
	# Set the correct locale before running Ansible to avoid encoding issues
	export LC_ALL=en_US.UTF-8; \
	export LANG=en_US.UTF-8; \
	ansible-playbook -i $(HOSTS_INI_FILE) $(K8S_ROOT_DIR)/debug.yml \
		--extra-vars "ROOT_DIR=$(ROOT_DIR)" --extra-vars $(EXTRA_VARS)

#### b. Provision k8s ####
k8s-install: k8s-rke2-install k8s-helm-install
k8s-uninstall: k8s-helm-uninstall k8s-rke2-uninstall

#### c. Provision rke2 ####
k8s-rke2-install:
	# Set the correct locale to avoid any potential encoding issue
	export LC_ALL=en_US.UTF-8; \
	export LANG=en_US.UTF-8; \
	ansible-playbook -i $(HOSTS_INI_FILE) $(K8S_ROOT_DIR)/rke2.yml --tags install \
		--extra-vars "ROOT_DIR=$(ROOT_DIR)" --extra-vars $(EXTRA_VARS)

k8s-rke2-uninstall:
	# Ensure proper locale is set
	export LC_ALL=en_US.UTF-8; \
	export LANG=en_US.UTF-8; \
	ansible-playbook -i $(HOSTS_INI_FILE) $(K8S_ROOT_DIR)/rke2.yml --tags uninstall \
		--extra-vars "ROOT_DIR=$(ROOT_DIR)" --extra-vars $(EXTRA_VARS)

#### d. Provision helm ####
k8s-helm-install:
	# Ensure the locale is correct before installing Helm
	export LC_ALL=en_US.UTF-8; \
	export LANG=en_US.UTF-8; \
	ansible-playbook -i $(HOSTS_INI_FILE) $(K8S_ROOT_DIR)/helm.yml --tags install \
		--extra-vars "ROOT_DIR=$(ROOT_DIR)" --extra-vars $(EXTRA_VARS)

k8s-helm-uninstall:
	# Set locale before uninstalling Helm
	export LC_ALL=en_US.UTF-8; \
	export LANG=en_US.UTF-8; \
	ansible-playbook -i $(HOSTS_INI_FILE) $(K8S_ROOT_DIR)/helm.yml --tags uninstall \
		--extra-vars "ROOT_DIR=$(ROOT_DIR)" --extra-vars $(EXTRA_VARS)
