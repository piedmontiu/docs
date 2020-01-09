# Regular Maintenance
Regular maintenance is very important. Updating modules, themes, dependencies, and core ensures that the websites will remain secure and efficient. This section is broken down into two categories:
- Custom upstream managed sites
	- [e4.piedmontu.edu](https://e4.piedmontu.edu)
	- [leadership.piedmontu.edu](https://leadership.piedmontu.edu)
	-  [education.piedmontu.edu](https://education.piedmontu.edu)
	- [business.piedmontu.edu](https://business.piedmontu.edu)
	- [sas.piedmontu.edu](https://sas.piedmontu.edu)
	- [seminary.piedmontu.edu](https://seminary.piedmontu.edu)
- Composer managed sites
	- [piedmontu.edu](https://piedmontu.edu)
	- [dashboard.piedmontu.edu](https://dashboard.piedmontu.edu)

## Required Tools
In order to do updates on the sites, you will need to have the following tools installed on your local development environment:
- [Git](https://git-scm.com/)
- [Composer](https://getcomposer.org/)
- [Terminus](https://pantheon.io/docs/terminus/install)

A basic understanding of how these tools are used is also essential to the rest of this section. Common commands and procedures will be documented, but these docs don’t go in-depth into how these tools work.

### Cloning Sites Locally
To get started managing any site, you will need to clone it to your development environment. Pantheon provides an easy way to do this. On the Pantheon dashboard, find a site and copy the command found under the **Clone with Git** button. Paste this into your terminal to copy the specified site locally.

## Custom Upstream Managed Sites
All changes made to the custom upstream should not be done in an individual site. Rather, they should be performed in `piu-theme`.
### Updating Modules and Themes
To get started with updating modules and themes on custom upstream managed sites you will need to have cloned the `piu-theme`  locally to your development environment. All module and theme updates should be made to this git repo instead of the individual sites. Making changes to this repo will allow all the microsites to pull from the same codebase.

When there is a module update, download the ZIP file from the Drupal.org website. Unzip the folder and drag it to `piu-theme/modules` to replace the existing folder for that module. The same process applies when installing a new module. For theme updates, download the ZIP file from Drupal.org, unzip the folder, and drag it to `piu-theme/themes/contrib`

### Updating Core
All the custom upstream managed sites have a further upstream of Pantheon’s Drupal 8 git bucket. That is where core updates come from. To update core, run the following commands:
```
$ git fetch pantheon-drops-8
$ git merge pantheon-drops-8/master
```
Do not attempt to copy in the official download from Drupal’s website. This will break Pantheon workflow functionality.

Provided no updates happened when running those core updates, proceed to commit the changes.
```
$ git commit -m ‘Update core to latest version’
```
Then, push the changes to the `piu-theme` bucket.
```
$ git push
```
To go one step further, you can run a Terminus command to automatically pull in the updates from the custom upstream and apply them to the dev environments of the respective sites. You will need the [Terminus Mass Update](https://github.com/pantheon-systems/terminus-mass-update) plugin to do so. To apply updates, run this command:
```
$ terminus site:list —format=list | terminus site:mass-update:apply
```
## Composer Managed Sites
The composer managed sites are done this way in order to install modules with advanced features that require PHP dependencies.

### Updating Modules and Themes
To update modules, themes, and core, `cd` into the local site directory and install updates for a specific module using this command: `$ composer update drupal/MODULE_NAME`

### Updating Core
Run this command `$ composer update drupal/core —with-dependencies` in the local site directory to update Drupal Core. Once this is done, commit the changes and push to the dev environment.

### Updating Dependencies
Simply run `$ composer update` and composer will check for any PHP dependencies that need updating and automatically proceed to update them. Following this step, commit the changes and push them to the dev environment.