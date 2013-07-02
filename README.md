# capistrano-typo3-cms

This is an extension for the deployment tool Capistrano. This extension facilitates the deploy of TYPO3 CMS projects.
A German introduction can be reached at http://goo.gl/6B9Cj

It includeds follows additional packages:

1. capistrano
- capistrano-ext
- railsless-deploy
- capistrano_colors

___

[![Build Status](https://travis-ci.org/programmerqeu/capistrano-typo3-cms.png?branch=major/0)](https://travis-ci.org/programmerqeu/capistrano-typo3-cms)
[![Gem Version](https://badge.fury.io/rb/capistrano-typo3-cms.png)](http://badge.fury.io/rb/capistrano-typo3-cms)
[![Dependency Status](https://gemnasium.com/programmerqeu/capistrano-typo3-cms.png)](https://gemnasium.com/programmerqeu/capistrano-typo3-cms)
[![Code Climate](https://codeclimate.com/github/programmerqeu/capistrano-typo3-cms.png)](https://codeclimate.com/github/programmerqeu/capistrano-typo3-cms)
[![Coverage Status](https://coveralls.io/repos/programmerqeu/capistrano-typo3-cms/badge.png?branch=master)](https://coveralls.io/r/programmerqeu/capistrano-typo3-cms)
[![githalytics.com alpha](https://cruel-carlota.pagodabox.com/19a3c56b0f7b6cbe4ae22b905477ce5e "githalytics.com")](http://githalytics.com/programmerqeu/capistrano-typo3-cms)

## Installation

Install it yourself wit rubygems package manager:
```bash
gem install capistrano-typo3-cms
```    

Need help to install ruby? http://docs.rubygems.org/read/chapter/3
Or easer with RVM: https://rvm.io/   

## Usage

### Configuration

**Capify** your project
```bash
$ cd /path/to/your/project
$ capify ./
```
Edit your **./Capfile** to include _capistrano-typo3-cms_

```ruby
load 'deploy' if respond_to?(:namespace)

require 'capistrano'
require 'capistrano-typo3-cms'
load 'config/deploy' # remove this line to skip loading any of the default 
```

Edit your ***./config/deploy.rb** in enable multistage support:

```bash
set :stages, %w(production staging)
set :default_stage, "staging"
require 'capistrano/ext/multistage'
 
set :application,			"your-prject-name"
set :user,				"www-data"
set :group,				"www-data"
 
 
# deploy strategy
set :deploy_via,			:rsync_with_remote_cache
set :scm, :git
set :copy_exclude,			".DS_Store", ".git/*", ".git", ".gitignore", ".gitmodules"]
 
 
# ssh & shell settings
set :use_sudo,				false
set :ssh_options,			{ :keys => %w(~/.ssh/id_dsa ~/.ssh/id_rsa) }
set :ssh_options,			{ :forward_agent => true }
set :ssh_local,				'ssh'
default_run_options[:shell]		= '/bin/bash'
set :php_cli,				"php"
_cset :php_cli,				"php"
 
 
# rsync  (rsync_with_remote_cache)
set :local_cache,			"cache/rsync"
set :rsync_options,			"-avz --delete --chmod=u=rwx,g=rx,o=r --exclude=*.svn* "
set :copy_exclude,			["REVERSION", ".svn", ".svn/*", ".git/*", ".git", ".gitignore", ".gitmodules"]
set :rsync_local,			"rsync"
 
# repository
set :repository,			"git@bitbucket.org:vergissberlin/#{application}.git"
set :git_enable_submodules,		1
#set :branch,                            "#{rails_env}"
set :branch,				"master"
 
# remote
set :keep_releases,			10
set(:deploy_to)				{ "/var/www/#{application}/#{rails_env}/deploy" }
set :deploy_via,			:remote_cache
set :rails_env,				"production"
```

Add your enviroments settings file at **./config/deploy/staging.rb** (for example)
```ruby
server 'staging.server.eu', :app, :web, :primary => true
set :rails_env, 'staging'
```



In case of using multistage create config files for every enviroment (deploy/staging.rb):
- http://capifony.org/cookbook/using-the-multistage-extension.html
- https://github.com/capistrano/capistrano/wiki/2.x-Multistage-Extension

### Deploy
	cap <enviroment> deploy:setup
	cap <enviroment> deploy


## Project roadmap
### Version 0.0.1

- Documentation "Deploying with Capistrano and TYPO3 CMS
- Resolving dependencies for PHP projects

### Version 1.0.0

Additional TYPO3:CMS tasks

	typo3:cms:setup
	typo3:cms:cc
	typo3:cms:check
	typo3:cms:data:push
	typo3:cms:data:pull
	typo3:cms:data:backup

### Version 2.0.0
Including new package "capistrano-mysql" to manage database data.

	mysql:push
	mysql:pull
	mysql:backup
	mysql:sync <source_env> <target_env>
	mysql:migrate
	
### Version 3.0.0
CLI command support to create a new TYPO3 CMS project from the scratch.
```bash
$ typo3 init ./
```

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
