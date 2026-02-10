# PHP & Symfony

- Contains setup and usage details related to legacy `symfony 3.3.16` framework running with `php 7.2`.

## Table of Contents

- [Setup \& Installation](#setup--installation)
  - [`php7.2`](#php72)
  - [Composer (1 and 2)](#composer-1-and-2)
  - [Creating a New Project in `symphony 3.3.16`](#creating-a-new-project-in-symphony-3316)
- [Run Webserver (Dev)](#run-webserver-dev)
- [Backend: Symfony Architecture \& Request Flow](#backend-symfony-architecture--request-flow)
- [Symfony Console (Basics)](#symfony-console-basics)

## Setup & Installation

> **NOTE**: Skip this step if setup and installation is already completed.

- Contains instructions for the following software packages and/or tools:
  - [PHP 7.1](#php72)
  - [Composer (1 and 2)](#composer-1-and-2)
  - [Creating a New Project in `symphony 3.3.16`](#creating-a-new-project-in-symphony-3316)

[:arrow_double_up:](#table-of-contents)

### `php7.2`

- The following software/tools will be required to start a `symfony 3.3.16` project using `php 7.2`:

  ```sh
  # Ensure that software common properties is installed
  sudo apt install software-properties-common -y

  # Add ondrej's php repository to ppa
  sudo add-apt-repository ppa:ondrej/php -y

  # Update the repositories list
  sudo apt update

  # Install PHP 7.2
  sudo apt install php7.2 php7.2-fpm php7.2-mysql php7.2-xml php7.2-mbstring php7.2-curl php7.2-gd -y
  ```

  In case you already have a version of PHP installed, you can change the default to point to `php7.2`:

  ```sh
  # Interactive usage
  # sudo update-alternatives --config php

  # Recommended direct setup
  sudo update-alternatives --set php /usr/bin/php7.2
  ```

[:arrow_double_up:](#table-of-contents)

### Composer (1 and 2)

- Download/Install `composer` (for legacy `symfony` projects, download and install `composer-1`, otherwise stick to `composer-2`):
  > For running `symfony 3.3.16` projects, `composer-1.x` with `php7.2` works best.
  - For legacy symfony projects, download [`composer-1.x`](https://getcomposer.org/composer-1.phar), and do the following

    ```sh
    # Create a local binaries directory
    sudo mkdir -p /usr/local/bin
    
    # Copy the PHP archive (.phar) file to /usr/local/bin
    sudo cp /path/to/composer-1.phar /usr/local/bin/composer1

    # Make the file executable
    sudo chmod +x /usr/local/bin/composer1

    # Create a symbolic link called /usr/local/bin/composer that points to /usr/local/bin/composer1
    sudo ln -s /usr/local/bin/composer1 /usr/local/bin/composer
    ```

    **NOTE**: In case there's a need to change to the latest version of `composer`, then create a new symbolic link in `/usr/local/bin/` and point it to the required binary.

  - For newer projects follow the [official guide](https://getcomposer.org/download/), or follow the following steps:
    - Download [`composer-2.x` (latest)](https://getcomposer.org/https://getcomposer.org/composer.phar), and follow the same steps as mentioned for `composer-1.x`, except this time, copying/moving/renaming the file would be as `/usr/local/bin/composer2`.

[:arrow_double_up:](#table-of-contents)

### Creating a New Project in `symphony 3.3.16`

- Create a `symfony 3.3.16` project:
  
  ```sh
  git clone --branch v3.3.16 https://github.com/symfony/symfony-standard.git my_project_name
  cd my_project_name

  # Ensure that composer is pointing to composer1 (not v2)
  composer install
  ```

  After running `composer install`, composer will prompt the user for some parameters for the project which will be important to set. The parameters are the following:

  ```terminal
  database_host (127.0.0.1):
  database_port (null): 
  database_name (symfony):
  database_user (root):  
  database_password (null):
  mailer_transport (smtp):
  mailer_host (127.0.0.1):
  mailer_user (null):
  mailer_password (null):
  secret (ThisTokenIsNotSoSecretChangeIt):
  ```

  > **NOTE**: It's a good practive to use user-specific values for `database_name`, `database_user`, and `database_password`.  Rest all can be configured later.

[:arrow_double_up:](#table-of-contents)

## Run Webserver (Dev)

> **NOTE**: Complete the steps in [Setup & Installation](#setup--installation), otherwise the server won't run.

- Run the webserver on `localhost` (by default, the server runs on port `8000`) from inside the created project:

  ```sh
  php bin/console server:run
  ```

- You should be able to open the server on <http://localhost:8000> (or <http://127.0.0.1:8000>).

[:arrow_double_up:](#table-of-contents)

## Backend: Symfony Architecture & Request Flow

![symfony-request-flow-dark](./img/symfony-req-res-flow-dark.svg)

[:arrow_double_up:](#table-of-contents)

## Symfony Console (Basics)

1. Typing in the following will list commands that can be used with `symfony console`:

   ```bash
   # From inside your /path/to/project, run:
   php bin/console

   # The shorter version of bin/console is using --help
   php bin/console --help
   ```

2. Clearing Cache

   ```bash
   php bin/console cache:clear

   # To know more about cache:clear command, a --help tag is useful
   php bin/console cache:clear --help

   # To clear production environment cache, use:
   php bin/console cache:clear --env=prod --no-debug
   ```

   This works for other commands as well.

[:arrow_double_up:](#table-of-contents)

## Bundles

### Bundle Basics

1. Everything starts at [`app/AppKernel.php`](./projects/carmakers/app/AppKernel.php) file, where all the bundles are register in `registerBundles` function.
2. By default, all the bundles are symfony standard bundles, except for [`AppBundle`](./projects/carmakers/src/AppBundle/) (mentioned in [`app/AppKernel.php#L18`](https://github.com/Ch-sriram/php-symfony/blob/1bf86911cb57e18e8501f109e29229f674280657/projects/carmakers/app/AppKernel.php#L18))
3. There are 2 kinds of configurations:
   1. The variable defined `$bundles` is just a regular bundle configuration in [`app/AppKernel.php#L10`](https://github.com/Ch-sriram/php-symfony/blob/1bf86911cb57e18e8501f109e29229f674280657/projects/carmakers/app/AppKernel.php#L10).
   2. Development & test based confifuration in [`app/AppKernel.php#L21-L30`](https://github.com/Ch-sriram/php-symfony/blob/1bf86911cb57e18e8501f109e29229f674280657/projects/carmakers/app/AppKernel.php#L21-L30).
4. The `AppBundle` defined in `app/AppKernel.php` is usually available inside [`src/AppBundle/AppBundle.php`](https://github.com/Ch-sriram/php-symfony/blob/1bf86911cb57e18e8501f109e29229f674280657/projects/carmakers/src/AppBundle/AppBundle.php), which just extends the standard `Bundle` class.
5. The `AppBundle` has a controller in [`src/AppBundle/Controller/DefaultController.php`](https://github.com/Ch-sriram/php-symfony/blob/1bf86911cb57e18e8501f109e29229f674280657/projects/carmakers/src/AppBundle/Controller/DefaultController.php), where a default template is rendered. All The views are stored in [`/app/Resources`](./projects/carmakers/app/Resources/).

[:arrow_double_up:](#table-of-contents)

### Bundle Generation

1. To generate a custom bundle, we need to use `symfony console`:

   ```bash
   # In your /path/to/project
   php bin/console generate:bundle
   ```

   And it generates the following prompts, which can be filled in as follows:

   ```terminal
                                              
     Welcome to the Symfony bundle generator!  
                                              

   Are you planning on sharing this bundle across multiple applications? [no]: 

   Your application code must be written in bundles. This command helps
   you generate them easily.

   Give your bundle a descriptive name, like BlogBundle.
   Bundle name: CarBundle

   Bundles are usually generated into the src/ directory. Unless you're
   doing something custom, hit enter to keep this default!

   Target Directory [src/]: 

   What format do you want to use for your generated configuration?

   Configuration format (annotation, yml, xml, php) [annotation]: 

                      
     Bundle generation  
                      

   > Generating a sample bundle skeleton into app/../src/CarBundle
     created ./app/../src/CarBundle/
     created ./app/../src/CarBundle/CarBundle.php
     created ./app/../src/CarBundle/Controller/
     created ./app/../src/CarBundle/Controller/DefaultController.php
     created ./app/../tests/CarBundle/Controller/
     created ./app/../tests/CarBundle/Controller/DefaultControllerTest.php
     created ./app/../src/CarBundle/Resources/views/Default/
     created ./app/../src/CarBundle/Resources/views/Default/index.html.twig
     created ./app/../src/CarBundle/Resources/config/
     created ./app/../src/CarBundle/Resources/config/services.yml
   > Checking that the bundle is autoloaded
   FAILED
   > Enabling the bundle inside app/AppKernel.php
     updated ./app/AppKernel.php
   OK
   > Importing the bundle's routes from the app/config/routing.yml file
     updated ./app/config/routing.yml
   OK
   > Importing the bundle's services.yml from the app/config/config.yml file
     updated ./app/config/config.yml
   OK

                                                                    
     The command was not able to configure everything automatically.  
     You'll need to make the following changes manually.              
                                                                    

   - Edit the composer.json file and register the bundle
     namespace in the "autoload" section:
   ```

   **NOTE**: In case there's an error when generating the bundle, then try and see what the error is.  In the case, the error is that `CarBundle` isn't added automatically to `"autoload"` section, to resolve it, just make the following changes to `composer.json`:
  
   ```diff
   index 3d16d65..5cf6e04 100644
   --- a/projects/carmakers/composer.json
   +++ b/projects/carmakers/composer.json
   @@ -5,7 +5,8 @@
       "description": "The \"Symfony Standard Edition\" distribution",
       "autoload": {
           "psr-4": {
   -            "AppBundle\\": "src/AppBundle"
   +            "AppBundle\\": "src/AppBundle",
   +            "CarBundle\\": "src/CarBundle"
           },
           "classmap": [ "app/AppKernel.php", "app/AppCache.php" ]
       },
   ```

2. Once this is run, the following happens:
   1. [`CarBundle`](./projects/carmakers/src/CarBundle/) gets generated inside [`src/`](./projects/carmakers/src/CarBundle/) with:
      - [`CarBundle.php`](./projects/carmakers/src/CarBundle/CarBundle.php) file, which just is empty, and extends the `Bundle` class.
      - [`Controller/`](./projects/carmakers/src/CarBundle/Controller/) directory that contains the [`DefaultController.php`](./projects/carmakers/src/CarBundle/Controller/DefaultController.php), which contains the `indexAction()` method.
      - [`Resources/`](./projects/carmakers/src/CarBundle/Resources/) directory that contains 2 more subdirectories that pertain to:
        1. [`Resources/config`](./projects/carmakers/src/CarBundle/Resources/config/) directory, that contains the configuration related to services inside `CarBundle`, inside the [`Resources/config/services.yml`](./projects/carmakers/src/CarBundle/Resources/config/services.yml) file.
        2. [`Resources/views`](./projects/carmakers/src/CarBundle/Resources/views/) directory, which can be subdivided into many other directories based on the user's requirement, but by default, a [`Resources/views/Default`](./projects/carmakers/src/CarBundle/Resources/views/Default/) directory is generated, inside which there's a Twig template which is named as [`index.html.twig`](./projects/carmakers/src/CarBundle/Resources/views/Default/index.html.twig).
   2. [AppKernel.php](./projects/carmakers/app/AppKernel.php) is updated as follows:

      ```diff
      @@ -16,6 +16,7 @@ class AppKernel extends Kernel
                   new Doctrine\Bundle\DoctrineBundle\DoctrineBundle(),
                   new Sensio\Bundle\FrameworkExtraBundle\SensioFrameworkExtraBundle(),
                   new AppBundle\AppBundle(),
      +            new CarBundle\CarBundle(),
                ];

                if (in_array($this->getEnvironment(), ['dev', 'test'], true)) {
      ```

   3. [`config.yml`](./projects/carmakers/app/config/config.yml) is updated, so that [`CarBundle's service.yml`](./projects/carmakers/src/CarBundle/Resources/config/services.yml) is recognized by `AppKernel`, and it's updated as follows:

      ```diff
      @@ -2,6 +2,7 @@ imports:
           - { resource: parameters.yml }
           - { resource: security.yml }
           - { resource: services.yml }
      +    - { resource: "@CarBundle/Resources/config/services.yml" }
        
       # Put parameters here that don't need to change on each machine where the app is deployed
       # https://symfony.com/doc/current/best_practices/configuration.html#application-related-configuration
      ```

   4. Routing is updated at [`routing.yml`](./projects/carmakers/app/config/routing.yml) as follows:

      ```diff
      @@ -1,3 +1,8 @@
      +car:
      +    resource: "@CarBundle/Controller/"
      +    type:     annotation
      +    prefix:   /
      +
      app:
          resource: '@AppBundle/Controller/'
          type: annotation
      ```

[:arrow_double_up:](#table-of-contents)

### Install & Configure 3rd Party Bundle

1. Third party bundles are not really part of the application.  It's located inside `/vendor` directory along with all the other `composer` dependencies.
2. Third party bundles can be installed using `composer`, and its source code originates from `packages`, `BitBucket`, `GitHub`, or any other place that `composer` can use.
3. Also, 3rd party bundles need not be something that needs to be downloaded from internet, you can build your own bundles, which can be shared across the team.
4. Where to find the bundles?  Google, ChatGPT, or Gemini for the requirement, and see what's being recommended.
   - Ex: `KnpMenuBundle` is one of the bundles that automatically generates menus, and can be used to route to specific templates.  To install the mentioned bundle, do the following:

     ```bash
     # Instead of `dev-master`, for symfony@3.3.16, find out the locked version, and use that for better compatibility
     composer require knplabs/knp-menu-bundle dev-master
     ```

   - The symfony application doesn't know anything about the new bundle, therefore, the `KnpMenuBundle` has to be added in `src/AppKernel.php`

[:arrow_double_up:](#table-of-contents)
