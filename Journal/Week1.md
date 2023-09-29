# Terraform Beginner Bootcamp 2023 - Week1

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

