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