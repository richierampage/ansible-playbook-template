# README #

This is a template Ansible playbook structure.
It is designed to be the base for new Ansible Playbook projects setting out a folder structure along the lines of Ansible best practices guidelines.
Any suggestions for improvements to this use the issue queue or tweet me @ibluebag

## Usage

Download this (as opposed to cloning it) to form the base of a new project.
Edit as required and update the readme specific to your project.
A sample base role is provided that you can use.
To create new roles use the following command in terminal within the roles directory.
````
ansible-galaxy init 'newrolename'
````
and that will create a role folder structure for you to use.

Ansible best practices can be read about here: http://docs.ansible.com/playbooks_best_practices.html

I strongly recommend purchasing a copy of the Ansible for Devops book by Jeff Geerling
https://leanpub.com/ansible-for-devops


## Running the playbook

Place details about running your playbook here


ansible-playbook playbook.yml -i hosts -u username -k -v

## Notes:
Using the {{ inventory_hostname variable }} vs the {{ ansible_hostname }}

## SSH

Adding these values to ~/.ssh/config will enable correctlty to vagrants using vagrant key

Host vm.autospray.dev
 IdentityFile /Users/rich/.vagrant.d/insecure_private_key
 IdentitiesOnly yes
 StrictHostKeyChecking no
 UserKnownHostsFile=/dev/null
 User vagrant
 LogLevel ERROR

Get Drupal Codebase when: drupal_env == "dev" }
 git_target_repo: "{{ git_source_repo }}"
 git_target_repo: "git@bitbucket.org:rmoger/{{ drupal_raw_domain }}.git"
 git_target_repo: "{{ git_source_repo }}"
 path={{ drupal_site_path }}
 Create {{ drupal_site_path }} directory
 path={{ drupal_site_path }}
 owner={{ remote_user }}
 group={{ remote_group }}
 url={{ drupal_makefile_url }}
 dest={{ drupal_site_path }}/{{ drupal_raw_domain }}.make
 build_method == 'makefile'
 command: '{{drush_location}} make {{ drupal_raw_domain }}.make -y'
 chdir: "{{ drupal_site_path }}"
 repo={{ git_target_repo }}
 dest={{ drupal_site_path }}
 version={{ drupal_site_git_branch }}
 command: '{{drush_location}} dl drupal-{{ drupal_major_version }} --destination="." -y --drupal-project-rename="{{ drupal_domain }}" '
 command: "rm -rf {{ drupal_site_path }}/.git/"

Set up aliases
 path="{{ drupal_site_path }}/sites/all/drush"
 src=templates/{{ drupal_env }}_vhost_template.j2
 dest="{{ drupal_site_path }}/sites/all/drush/{{ raw_domain }}.aliases.drushrc.php"

src=templates/site_aliases.j2
 'remote-host' => '{{ hostvars[host].hostname }}',
 'remote-user' => '{{ hostvars[host].remote_user }}',
 'root' => '{{ hostvars[host].drupal_site_path }}',
 'uri' => 'http://{{ hostvars[host].drupal_domain }}',

 'remote-host' => '{{ drush_source_hostname }}',
 'remote-user' => '{{ drush_source_remote_user }}',
 'root' => '{{ drush_source_root }}',
 'uri' => 'http://{{ drush_source_uri }}',

Set up bitbucket
 url: https://api.bitbucket.org/2.0/repositories/rmoger/{{ raw_domain }}
 chdir: "{{ drupal_site_path }}"

Install drupal
 stat: path="{{ drupal_site_path }}/sites/default/settings.php"
 command: "{{drush_location}}  si -y --site-name='{{ drupal_site_name }}' --account-name='{{ drupal_account_name }}' --account-pass='{{ drupal_account_pass }}' --db-url=mysql://{{ drupal_mysql_user }}:{{ drupal_mysql_password }}@localhost/{{ drupal_mysql_database }}"
 chdir: "{{ drupal_site_path }}"

Set Site Permissions
  file: path="{{ drupal_site_path }}" owner="rich" group="www-data" recurse=yes

Copy Site
  command: "drush -y rsync @{{ raw_domain }}.source:%files @self:%files "
  chdir: "{{ drupal_site_path }}"
  chdir: "{{ drupal_site_path }}"


## Running playbook

 ansible-playbook playbook.yml --limit vm.autospray.dev

 runs playbook.yml and limits to  vm.autospray.dev host


## New site

##Variables

new_site
    hosts: "{{ site }}"
    remote_user: "{{ remote_user }}"
    new site - drupal_group_vars: "{{ groups[raw_domain]

set_up_vhosts
    stat: path=/etc/apache2/sites-available/{{ drupal_domain }}.conf
    src=templates/{{ drupal_env }}_vhost_template.j2
    dest=/etc/apache2/sites-available/{{ drupal_domain }}.conf
    name: a2ensite {{ drupal_domain }}.conf
    command: a2ensite {{ drupal_domain }}.conf

setup mysql
    login_user={{ mysql_admin_user }}
    login_password={{ mysql_admin_password }}
    db={{ drupal_mysql_database }}
    login_user={{ mysql_admin_user }}
    login_password={{ mysql_admin_password }}
    name={{ drupal_mysql_user }}
    host={{ item }}
    priv={{ drupal_mysql_database }}.*:ALL
    password={{ drupal_mysql_password }}










drupal_domain


  Creates site based on template and restarts apache set to p3 at present

  ansible-playbook new_site.yml -e "site=www.test1.co.uk" -vvvv


  create from existing repo using existing repo
  create from existing repo and crete new repo

  create from makefile using existing repo
  create from existing repo and crete new repo

  create from codebase
  create from existing repo and crete new repo

## Remove site

ansible-playbook nuke_site.yml -e "site=www.test1.co.uk"


## Sync db & Files

  ansible-playbook livetostage.yml -e "domain=www.testsite.co.uk" -vvvv