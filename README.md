# ASKII Party


## Local development prerequisites

You must have the following installed on your Mac OS X system in order to use this repository:

* Composer - [installation](https://getcomposer.org/doc/00-intro.md#globally-on-osx-via-homebrew-)
* Vagrant - [installation](https://docs.vagrantup.com/v2/installation/)
* Vagrant Triggers plugin - `vagrant plugin install vagrant-triggers`
* Ansible - [installation](http://docs.ansible.com/intro_installation.html)
* Virtualbox - [docs](https://www.virtualbox.org/)
* Git - derp of course
* Node/NPM
* Bower

## Setting up

1. Install codebase with `git clone git@github.com:reidelliott/askii.party.git`. Then cd into `askii` folder.
2. Copy SSH key to server with `ssh-copy-id askii@70.32.80.32` (must have password for user - find in Zoho Vault)
3. Get downloads folder with `scp -r askii@70.32.80.32:~/httpdocs/production/shared/web/app/uploads/* web/app/uploads/
4. Get database from production by logging into Plesk for the server and grabbing an export - save and unzip to `web/mysqldumps/output_to_dev.sql`
5. Install dependencies:

    ```
    $ composer install
    $ bundle install
    $ cd web/app/themes/askii
    $ npm install
    $ bower install
    ```
6. Spin up local dev server - make sure you are back in main project root folder and run `vagrant up`. Depending on your system, it may or may not prompt you if you want to source the database from `web/mysqldumps/output_to_dev.sql`. If it does prompt you, then skip to step 8. If it does not prompt you, do step 7.
7. In your web browser, go to http://askii.dev/phpmyadmin. Import the database dump you saved in `web/mysqldumps/output_to_dev.sql`.
8. Now back on the command line, get into the vagrant environment with `vagrant ssh && cd /var/www/`.
9. Run a search and replace on the database with `wp search-replace 'http://askii.party' 'http://askii.dev' --precise --all-tables` and then `wp search-replace 'askii.party' 'askii.dev' --precise --all-tables` just to be sure we don't miss anything.
10. Now back in your web browser you should be able to get to the site at http://askii.dev.
11. Give yourself a high five.

## Developing

This project uses gulp to compile assets. In `web/app/themes/askii` work with css/js/image files in the `assets` folder. Before you start working, run `gulp watch` to spin up the compiler and browsersync. When you make changes to any files in the `assets` folder, it will compile them down to the `dist` folder.
