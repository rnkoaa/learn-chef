# README.md

If you encounter an error installing vb-guest

VAGRANT_DISABLE_STRICT_DEPENDENCY_ENFORCEMENT=1 vagrant plugin install vagrant-vbguest

## Setup Chef Workstation

Use instructions from [Chef Instructions](https://docs.chef.io/install_dk.html#review-prerequisites)

create a cookbook

```sh
chef generate cookbook cookbooks/learn_chef_httpd
```

create a template

```sh
chef generate template cookbooks/learn_chef_httpd index.html
```

Run a recipe inside a cookbook

```sh
sudo chef-client --local-mode --runlist 'recipe[learn_chef_httpd]'
```

## Uploading a cookbook

1 create cookbook

    ```sh
    chef generate cookbook cookbooks/learn_chef_httpd
    ```

2 check the list of cookbooks

    ```sh
    knife cookbook list
    ```
3 Upload the cookbook you have created

    ```sh
    knife cookbook upload learn_chef_apache2
    ```

## Allowing two vagrant instances to communicate

1. generate a new ssh key in the first instance

```sh
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

2. copy the public key to the second instance

```sh
cat ~/.ssh/id_rsa.pub
```

3. On the second instance, create `~/.ssh/authorized_keys` if it does not exist
4. Paste the contents of the public ssh key to this file.
5. Save the file and change it to readonly 
    
    ```sh
    chmod 600 ~/.ssh/authorized_keys
    ```

## Bootstraping with chef

```sh
knife bootstrap 192.168.33.12 --ssh-user vagrant --sudo --identity-file ~/.ssh/id_rsa --node-name node1-centos --run-list 'recipe[learn_chef_apache2]'
```

```sh
knife bootstrap 192.168.33.12 --ssh-user vagrant --sudo --identity-file ~/.ssh/id_rsa --node-name node1-centos --run-list 'recipe[learn_chef_httpd]'
```

## To update a node or nodes after a new cookbook has been uploaded

login to the node and run

```sh
sudo chef-client
```

or

```sh
knife ssh 'name:node1-centos' 'sudo chef-client' --ssh-user vagrant  --identity-file ~/.ssh/id_rsa --attribute ipaddress
```