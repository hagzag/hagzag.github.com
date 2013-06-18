---
layout: post
title: "Managing drupal with Drush (Drupal Shell)"
date: 2010-08-27 09:18
comments: true
categories: [Drupal, Drush ]
---

{% img left /assets/images/drush.png 'Drush Logo' %}Well, those of you who know me now I've become a very big Drupal fan and one of the tools that will make your life much easier is [Drush][0]. Drush short for Drupal Shell and it works just like one.

Download [Drush][0] and add it to your path - assuming you are using unix/linux (if you are using Windows then a [tutorial can be found here][1] it actually works I've tried it). 

Drush dl will download latest stable Drupal to the current location ($PWD) and you are ready for your Drupal installation - OK so the bootstrap process isn't that simple because you still need to setup your environment (http, php, mysql/some other supported DB) but still it takes are of the downloading part making sure you are using the latest version etc. 

The real power in Drush comes when you have a large site with a bundle of distributed modules, and you need to start managing dependencies and versions. The problem with the standard way, http://yoursite/admin/build/modules, is: 

- It takes a year to load the page 

- In order to solve dependencies you need to: 
    
1.  Take a note of what you need to download
1.  Go to http://drupal.org/project/modules
1.  Search and download
1.  Upload to your server

This seems like to much work - Drush does all that for you, just cd to your Drupal installation directory run _**drush dl <module name\>**_ and Drush will get it and extract it for you under your sites/default/modules directory. 

Another bonus is you don't get stuck with those tar.gz files you need to manually remove after enabling the module. 

Thats not all, _**drush en <module name\>**_ & _**drush up <module name\> **_will enable the module (again no need to navigate to sites/all/modules and use the check-box. 

If you run _**drush up** _with no args Drush will attempt to update your Drupal installation and all the modules that need updates - be careful here for you do not want to break anything, for the upgrade includes your database tables !!!._  
_ 

The last "bonus" is that Drush stores all the updated modules / files in <site\_root\>/backup so you have all the old / updated data backed up in case you need to rollback (been there done that). 

For the full list of what drush can do for you - for exmaple if you rn multipul sites from the same drupal case and other options .. see drush help:

`_[me@mybox modules]# drush --help  
Execute a drush command. Run \`drush help [command]\` to view command-specific help.  
  
Examples:  
 drush dl cck zen                          Download CCK module and Zen theme.                    
 drush --uri=http://example.com status     Show status command for the example.com multi-site.   
 drush help --pipe                         A space delimited list of commands                    
  
Options:  
 -r <path>, --root=<path>                  Drupal root directory to use (default: current directory)                  
 -l <uri>, --uri=http://example.com        URI of the drupal site to use (only needed in multisite environments)      
 -v, --verbose                             Display extra information about the command.                               
 -d, --debug                               Display even more information, including internal messages.                
 -q, --quiet                               Hide all output                                                            
 -y, --yes                                 Assume 'yes' as answer to all prompts                                      
 -n, --no                                  Assume 'no' as answer to all prompts                                       
 -s, --simulate                            Simulate all relevant actions (don't actually change the system)           
 -i, --include                             A list of paths to search for drush commands                               
 -c, --config                              Specify a config file to use. See example.drushrc.php                      
 -u, --user                                Specify a user to login with. May be a name or a number.                   
 -b, --backend                             Hide all output and return structured data (internal use only).            
 -p, --pipe                                Emit a compact representation of the command for scripting.                
 --nocolor                                 Suppress color highlighting on log messages.                               
 --show-passwords                          Show database passwords in commands that display connection information.   
 -h, --help                                This help system.                                                          
 --php                                     The absolute path to your PHP intepreter, if not 'php' in the path. _`

[0]: http://drupal.org/project/drush
[1]: http://drupal.org/node/330023