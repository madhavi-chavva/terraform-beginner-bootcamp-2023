# Terraform Beginner Bootcamp 2023 - Week2

## Week2 TerraTown Provider Physical Diagram

![TerraTown Provider](https://github.com/madhavi-chavva/terraform-beginner-bootcamp-2023/assets/125069098/7438c402-8331-4232-b2af-2096d3f4af39)

## Working with Ruby

### Bundler

Bundler is a package manager for ruby.
It is the primary way to install ruby packages (known as gems) for ruby.

#### Install Gems

You need to create a Gemfile and define your gems in that file.

```rb
source "https://rubygems.org"

gem 'sinatra'
gem 'rake'
gem 'pry'
gem 'puma'
gem 'activerecord'
```
Then you need to run the `bundle install` command

This will install the gems on the system globally (unlike nodejs which install packages in place in a folder called node_modules)

A Gemfile.lock will be created to lock down the gem versions used in this project.

#### Executing ruby scripts in the context of bundler

We have to use `bundle exec` to tell future ruby scripts to use the gems we installed. This is the way we set context.

### Sinatra

Sinatra is a micro web-framework for ruby to build web-apps.

Its great for mock or development servers or for very simple projects.

You can create a web-server in a single file.

[sinatra_webserver](https://sinatrarb.com/)

## Terratowns Mock Server

### Running the web server

We can run the web server by executing the following commands:

```rb
bundle install
bundle exec ruby server.rb
```

All of the code for our server is stored in the `server.rb` file.

`Stop the web server` use `ctrl+C` to start the web server  `bundle exec ruby server.rb`

### Create(POST) a new HTTP request to web server.

```sh
./bin/terratowns/create
```
![create](https://github.com/madhavi-chavva/terraform-beginner-bootcamp-2023/assets/125069098/aa0136ed-9c76-4bf0-afad-bbc947891b71)

### Read(GET) a HTTP request to web server.

```sh
./bin/terratowns/read 897cc815-0fc2-48e2-bbce-53c5cb0c8575
```
![read](https://github.com/madhavi-chavva/terraform-beginner-bootcamp-2023/assets/125069098/cd60f1b4-ec8a-44eb-b478-746e0d173ff7)

### update(PUT) a HTTP request to web server.
```sh
./bin/terratowns/update 897cc815-0fc2-48e2-bbce-53c5cb0c8575
```
It was giving an error.
![update](https://github.com/madhavi-chavva/terraform-beginner-bootcamp-2023/assets/125069098/099d2c5d-c1b5-45ca-b0c1-98ad4b06ded3)

Modified the code using debugger `binding.pry`

![image](https://github.com/madhavi-chavva/terraform-beginner-bootcamp-2023/assets/125069098/34158355-cd63-4d06-bcac-65be1d15b548)

![image](https://github.com/madhavi-chavva/terraform-beginner-bootcamp-2023/assets/125069098/155b7bf7-84f7-42dc-8930-d3d501f6df2c)

re-tested it now it is working fine.
![update](https://github.com/madhavi-chavva/terraform-beginner-bootcamp-2023/assets/125069098/f5cecf01-d37f-4821-a6eb-b5e6724469c2)

### Delete(DELETE) a HTTP request to web server.
```sh
./bin/terratowns/DELETE 897cc815-0fc2-48e2-bbce-53c5cb0c8575
```
![delete](https://github.com/madhavi-chavva/terraform-beginner-bootcamp-2023/assets/125069098/c42096e0-f0ba-4ff2-80ac-2cb7a54dfd91)

It was not displaying the uuid so we modified the code in the DELETE 
![code](https://github.com/madhavi-chavva/terraform-beginner-bootcamp-2023/assets/125069098/6278ee96-1250-4531-b2cb-5519048c885e)

re-tested it.
![image](https://github.com/madhavi-chavva/terraform-beginner-bootcamp-2023/assets/125069098/e09f2aad-4d26-4c0e-8c16-7c54c2bca5f1)

## Diagram of HTTP Request to HTTP server

![HTTP Request to HTTP server](https://github.com/madhavi-chavva/terraform-beginner-bootcamp-2023/assets/125069098/c29e2cca-a269-4c66-b643-7c759db88d2e)


