# Terraform Beginner Bootcamp 2023 - Week1

## Fixing Tags

[How to Delete Local and Remote Tags on Git](https://devconnected.com/how-to-delete-local-and-remote-tags-on-git/)

Locall delete a tag
```sh
git tag -d <tag_name>
```

Remotely delete tag

```sh
git push --delete origin tagname
```

Checkout the commit that you want to retag. Grab the sha from your Github history.

```sh
git checkout <SHA>
git tag M.M.P
git push --tags
git checkout main
```

## Root Module Structure

Our root module structure is as follows:

```
PROJECT_ROOT
│
├── main.tf                 # everything else.
├── variables.tf            # stores the structure of input variables
├── terraform.tfvars        # the data of variables we want to load into our terraform project
├── providers.tf            # defined required providers and their configuration
├── outputs.tf              # stores our outputs
└── README.md               # required for root modules
```

[Standard Module Structure](https://developer.hashicorp.com/terraform/language/modules/develop/structure)

## Terraform and Input Variables

### Terraform Cloud Variables

In terraform we can set two kind of variables:
- Enviroment Variables - those you would set in your bash terminal eg. AWS credentials
- Terraform Variables - those that you would normally set in your tfvars file

We can set Terraform Cloud variables to be sensitive so they are not shown visibliy in the UI.

### Loading Terraform Input Variables

[Terraform Input Variables](https://developer.hashicorp.com/terraform/language/values/variables)

### var flag
We can use the `-var` flag to set an input variable or override a variable in the tfvars file eg. `terraform -var user_ud="my-user_id"`

### var-file flag

Create a file named `myvars.tfvars`

```
region        = "us-west-2"
ami           = "ami-0c55b159cbfafe1f0"
instance_type = "t2.micro"
ssh_key       = "my-key-pair"
```
Then apply using the -var-file option:
```
terraform apply -var-file=myvars.tfvars
```
In the -var-file approach, you can organize your variables in a separate file, making it easier to manage and reuse configurations. This is especially useful when you have a lot of variables or when you want to reuse the same set of variables across multiple Terraform runs.

### terraform.tvfars

This is the default file to load in terraform variables in blunk

### auto.tfvars

Create file with any name but extension should be `.auto.tfvars' for eg myvars.auto.tfvars

```
region        = "us-west-2"
ami           = "ami-0c55b159cbfafe1f0"
instance_type = "t2.micro"
ssh_key       = "my-key-pair"
```
- These values will be automatically loaded by Terraform when you run terraform apply in the same directory.
- If you don't provide values for these variables through other means (like command-line options or environment variables), Terraform will use the values 
 from auto.tfvars.
When you run `terraform apply`, Terraform will automatically use the values specified in auto.tfvars for the respective variables unless overridden by other configurations. This makes it convenient for setting default values, especially when working in a single directory.


### order of terraform variables

Terraform loads variables in the following order, with later sources taking precedence over earlier ones:

Environment variables
The terraform.tfvars file, if present.
The terraform.tfvars.json file, if present.
Any *.auto.tfvars or *.auto.tfvars.json files, processed in lexical order of their filenames.
Any -var and -var-file options on the command line, in the order they are provided. (This includes variables set by a Terraform Cloud workspace.)

![terraform_var file_precedence](https://github.com/madhavi-chavva/terraform-beginner-bootcamp-2023/assets/125069098/c6ae62c7-1ecf-4d77-823e-a0776ec08e6f)

## Dealing With Configuration Drift

## What happens if we lose our state file?

If you lose your statefile, you most likley have to tear down all your cloud infrastructure manually.

You can use terraform import but it won't for all cloud resources. You need check the terraform providers documentation for which resources support import.

### Fix Missing Resources with Terraform Import

`terraform import aws_s3_bucket.bucket bucket-name`

[Terraform Import](https://developer.hashicorp.com/terraform/cli/import)
[AWS S3 Bucket Import](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket#import)

### Fix Manual Configuration

If someone goes and delete or modifies cloud resource manually through ClickOps. 

If we run Terraform plan is with attempt to put our infrstraucture back into the expected state fixing Configuration Drift

## Fix using Terraform Refresh

```sh
terraform apply -refresh-only -auto-approve
```

## Terraform Modules

### Terraform Module Structure

It is recommend to place modules in a `modules` directory when locally developing modules but you can name it whatever you like.

### Passing Input Variables

We can pass input variables to our module.
The module has to declare the terraform variables in its own variables.tf

```tf
module "terrahouse_aws" {
  source = "./modules/terrahouse_aws"
  user_uuid = var.user_uuid
  bucket_name = var.bucket_name
}
```

### Modules Sources

Using the source we can import the module from various places eg:
- locally
- Github
- Terraform Registry

```tf
module "terrahouse_aws" {
  source = "./modules/terrahouse_aws"
}
```


[Modules Sources](https://developer.hashicorp.com/terraform/language/modules/sources)

## Considerations when using ChatGPT to write Terraform

LLMs such as ChatGPT may not be trained on the latest documentation or information about Terraform.

It may likely produce older examples that could be deprecated. Often affecting providers.

## Working with Files in Terraform


### Fileexists function

This is a built in terraform function to check the existance of a file.

```tf
condition = fileexists(var.error_html_filepath)
```

https://developer.hashicorp.com/terraform/language/functions/fileexists

### Filemd5

https://developer.hashicorp.com/terraform/language/functions/filemd5

### Path Variable

In terraform there is a special variable called `path` that allows us to reference local paths:
- path.module = get the path for the current module
- path.root = get the path for the root module
[Special Path Variable](https://developer.hashicorp.com/terraform/language/expressions/references#filesystem-and-workspace-info)

```
resource "aws_s3_object" "index_html" {
  bucket = aws_s3_bucket.website_bucket.bucket
  key    = "index.html"
  source = "${path.root}/public/index.html"
}

```
