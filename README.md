# Ansible Templating Demo
This project introduces Ansible Templating to new or intermediate experience levels. It was prepared for the Ansible-Indianapolis meetup.

# Prerequisites

1. Ansible, git and python must be installed on the Control machine.

    **Mac**  
    1. Install homebrew and use it to install the required packages.  
    ```bash  
    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"  
    brew install git python ansible  
    ```  

    **Linux**  
    1. Install required system packages.
    ```bash  
    yum install epel-release libffi-devel python-devel openssl-devel gcc sshpass  
    # For Debian systems, use:  
    # apt install libffi-dev python-dev libssl-dev gcc sshpass  
    ```

    2. Install pip and git.  
    ```bash  
    yum install python-pip git ansible
    # For Debian systems, replace yum with apt  
    ```

2. Create virtual environment to work in.
    ```bash  
    pip install virtualenv  
    # virtualenv will make a folder for you  
    virtualenv ~/path/to/project  
    cd ~/path/to/project  
    ```

3. Clone the ansible_templating_demo repo into the project folder.
    ```bash  
    git clone https://github.com/clearobject/ansible_templating_demo.git  
    ```

You should be ready to go! (did somebody just clone the readme from another repo???)

# Ansible Templating (Jinja2)

Ansible uses Jinja2 templating to enable dynamic expressions and access to variables.  Jinja2 is a modern and designer-friendly templating language for Python, modelled after Django’s templates. It is fast, widely used and secure with the optional sandboxed template execution environment.

From Jinja2 Template Designer Documentation:
> A Jinja template is simply a text file. Jinja can generate any text-based format (HTML, XML, CSV, LaTeX, etc.). A Jinja template doesn’t need to have a specific extension: .html, .xml, or any other extension is just fine.
> 
> A template contains variables and/or expressions, which get replaced with values when a template is rendered; and tags, which control the logic of the template. The template syntax is heavily inspired by Django and Python.

## Filters

Filters are used for transforming data inside a template expression. Jinja2 ships with many filters. See [builtin filters](http://jinja.pocoo.org/docs/2.9/templates/#builtin-filters) in the official Jinja2 template documentation for a full listing.  In addition the ones provided by Jinja2, Ansible ships with it’s own and allows users to add their own custom filters.  See [Ansible Filters](http://docs.ansible.com/ansible/latest/playbooks_filters.html) for this listing.

__Note:__
_Templating happens on the Ansible controller, __not__ on the task’s target host, so filters also execute on the controller as they manipulate local data._

## Tests

Tests in Jinja2 are a way of evaluating template expressions and returning True or False. Jinja2 ships with many of these. See builtin tests in the official Jinja2 template documentation. Tests are very similar to filters and are used mostly the same way, but they can also be used in list processing filters, like C(map()) and C(select()) to choose items in the list.  In addition to those Jinja2 tests, Ansible supplies a [few more](http://docs.ansible.com/ansible/latest/playbooks_tests.html) and users can easily create their own.

__Note:__
_Like all templating, tests always execute on the Ansible controller, __not__ on the target of a task, as they test local data._

## Examples

#### basic string manipulation

| Example        | What it does           | Times incountered in our automation  |
| -------------- |:-------------:| -----:|
| {{ variable_name \| upper() }} | _converts text value of variable to upper case_  | 20 |
| {{ variable_name \| lower() }} | _converts text value of variable to lower case_ | 13 |
| {{ variable_name \| capitalize() }} | _adds capitalization for text value of variable_ | 4 |
| {{ variable_name \| uri_split(0) }} | _pulls data out of a URI formatted value_ | 2 |
| {{ variable_name \| urlencode }} | _applies url encoding to value_ | 1 |
| {{ variable_name \| password_hash('sha512') }} | _hashes a password value_ | 1 |
| {{ variable_name \| to_json }} or {{ variable_name \| to_nice_json }} | _renders value as JSON_ | 3 |

#### basic number manipulation

| Example        | What it does           | Times incountered in our automation  |
| -------------- |:-------------:| -----:|
| {{ variable_name \| round }} | _round to nearest whole number_ | 4 |
| {{ variable_name \| min }} | _get the minimum value from list of numbers_ | 2 |

#### variable control

| Example        | What it does           | Times incountered in our automation  |
| -------------- |:-------------:| -----:|
| {{ variable_name \| default(true) }} | _provides a default value inline for variable_ | 44 |
| {{ variable_name \| mandatory }} | _require variable inline_ | 1 |
| {{ variable_name \| int }} | _explicit type conversion to integer_ | 4 |


