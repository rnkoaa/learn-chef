To use this to create a chef learning environment
1. Generate an ssh key which will be used for a node-to-node communication
```sh
 ssh-keygen -t rsa -b 4096 -C "vagrant@vagrant.com"
```
Do not add any secret to this key. Set the location as `./data/ssh`
2. Run `vagrant up` to create both instances.

1. Go to [Chef Manager](manage.chef.io)
2. Download starter kit to your data volume for workstation
3. `vagrant ssh workstation` to ssh into the workstation
4. Copy the chef-starter.zip from `/vagrant_data/chef-starter.zip` to `~/chef-starter.zip`
5. Unzip the chef-starter.zip file and delete the zip.
  1. `unzip chef-starter.zip && rm chef-starter.zip`
6. change into the chef-repo and check if everything is good
  1. `cd chef-repo`
  2. `knife client list`
  3. `knife node list`


## Deleting nodes and clients

```sh
knife node delete --yes NODENAME
knife client delete --yes NODENAME
```

## Bootstrapping a Node with knife

```sh
# example knife command
# knife bootstrap fqdn/ip --sudo \
#     -x, --ssh-user USERNAME
#     -i, --ssh-identity-file
#     -P, --ssh-password  PASSWORD
#     -p, --ssh-port    PORT
#     -N, --node-name   NODENAME
#     -r, --run-list    RUN_LIST (optional)
knife bootstrap --sudo 192.168.33.20 -x vagrant -p 22 -i ~/.ssh/id_rsa -N chef_node_1
```

## Logic in Cookbooks
Create a new file in Attributes directory called `defaults.rb` located at `cookbooks/apache/attributes/defaults.rb`

```ruby
default["package_name"] = "apache2"
default["service_name"] = "apache2"
default["document_root"] = "/var/www"

if node["platform"] == "centos"
  default["package_name"] = "httpd"
  default["service_name"] = "httpd"
  default["document_root"] = "/var/www/html"
end
```

For a better Implementation we can use `case statement` such as below
```ruby
case node["platform"]
when "ubuntu"
  default["package_name"] = "apache2"
  default["service_name"] = "apache2"
  default["document_root"] = "/var/www"
when "centos"
  default["package_name"] = "httpd"
  default["service_name"] = "httpd"
  default["document_root"] = "/var/www/html"
end
```

Additionally, instead of using `node platform`, we can extend the range of environments
where the cookbook to run by using `node[platform_family]` eg. `rhel`

Now in the `recipes/default.rb` file, we can create the cookbooks as follows
```ruby
  package node["package_name"] do
    action :install
  end

  service node["package_name"] do
    action [:enable, :start]
  end

  template "#{node["document_root"]}/index.html" do
    source "index.html.erb"
    mode "0644"
  end
```
