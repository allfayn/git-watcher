## Introduction

Git Watcher is a multi-platform desktop app written in pure HTML and Javascript using node-webkit.

It shows diff information about local staged/unstaged files and allows you to commit changes. UI is updated in real-time by detecting file changes and git index changes. Submodules also inform changes to their parent module.

It also organizes repository submodules in tabs, so that you can work easily with them without the need of having multiple git gui instances or shells.

In my opinion, the native git gui app is awful and lacks a lot of features. This app aims to outstand it :)

## Features

* **Real-time** multiple file diff information with line numbers and **syntax highlighting**
* Allows you to work with **submodules organized in tabs**
* Support for **hunk and line staging**
* Allows you to **open files** by clicking on its name or lines numbers. You can even use your preferred editor!
* Shows current **branch information**: upstream branch and ahead/behind commit count
* Menu bar with **shortcuts, configuration options and utilities**
* System Tray support
* Support for custom tools (external commands)

## Screenshots (v0.5.0)
![Overview 1](http://i.imgur.com/D84jkKK.png)
![Overview 2](http://i.imgur.com/foXpFd7.png)
![Overview 3](http://i.imgur.com/PMqHodV.png)
![Overview 4](http://i.imgur.com/ps73SBC.png)
![Overview 5](http://i.imgur.com/iZAVXyq.png)
![Overview 6](http://i.imgur.com/vaZfPpz.png)

## Download

Please consider downloading the proper build according to your distribution. 
See __Troubleshooting__ if you cannot run the app.

* Linux x64 - __Old dists__: Ubuntu 12.04, 12.10 or derivative distributions
	* [v0.4.11](https://bitbucket.org/demian85/git-watcher/downloads/gitw-linux-x64-v0.4.11.tar.gz)
	* [v0.5.0](https://bitbucket.org/demian85/git-watcher/downloads/gitw-linux-x64-v0.5.0.tar.gz)
* Linux x64 - __New dists__: Ubuntu 13.04+, Gentoo, Arch, Fedora 18+
	* [v0.4.11](https://bitbucket.org/demian85/git-watcher/downloads/gitw-linux-x64-new-v0.4.11.tar.gz)
	* [v0.5.0](https://bitbucket.org/demian85/git-watcher/downloads/gitw-linux-x64-new-v0.5.0.tar.gz)

**Someone please help me to create Windows & Mac binaries!**

## How to run the app

Just extract file contents and execute `./gitw`.

You can also:

* Create a desktop shortcut :P
* `sudo npm link`. A link to the app will be created in `/usr/local/bin`, so you can run the app using `gitw` command. If it's a valid Git repository, the current working directory is used by default.

## Configuration options

The application stores configuration data as a JSON file in `~./config/gitw/config.json`.
Certain config values can be modified using the app `options` menu. Until the UI provides a complete way of customizing these values, you can edit the file yourself.

Current config file structure EXAMPLE:

```Javascript
{
	"defaultRepository": "~/www/myproject",    // default git repository to load on startup
	"debugMode": false,    // enable debugging
	"diff": {
		"contextLines": 4,
		"ignoreEolWhitespace": true,
		"highlight": {
			"enabled": true,
			"byFileExtension": {    // enable/disable syntax highlighting by file extension
				".js": true,
				".php": true
			}
		}
	},
	"external": {    // custom commands
		"fileOpener": {    // handle file opening, empty path uses system default application
			"path": "/usr/local/netbeans-7.4/bin/netbeans",
			"args": ["--open", "$FILE:$LINE"]    // $FILE is replaced by the file path. $LINE is replaced by line number (if available)
		},
		"directoryOpener": {    // handle directory opening, empty path uses system default application
			"path": "",
			"args": []
		}
	},
	"tools": [	// custom tools to be included in the menu bar under the "Tools" menu
		{
			"name": "My custom command",
			"cmd": "/path/to/script.sh",
			"args": []
		}
	],
	"shortcuts": {	// customize shortcuts, you can combine the following modifiers with a letter: cmd, shift, ctrl, alt
		"repositoryOpen": "ctrl+alt o",
		"repositoryClose": "ctrl+alt c",
		"repositorySubmoduleUpdate": "ctrl+alt u",
		"repositoryExplore": "ctrl+alt e",
		"repositoryBrowse": "ctrl+alt k",
		"repositoryRefresh": "F5",	// F1-F11 keys are allowed. F12 is reserved for devtools
		"repositoryQuit": "ctrl q",
		"branchCreate": "ctrl n",
		"branchCheckout": "ctrl o",
		"branchDelete": "ctrl d",
		"stashSave": "ctrl s",
		"stashPop": "ctrl p"
	}
}
```

## TODO

I'm working on the following features
(your help will be much appreciated!)

* Amend commit (Really hard to do!)
* Rename branches
* "File" menu with shortcuts
* Delete, edit and add remotes
* More configuration options
* Better syntax highlighting.
* UI improvements: allow to view diff one file at a time to avoid "confusion".
* Hooks/tools: allow to add external commands to interact with branches or files.

## Create your own build

Please read [node-webkit wiki](https://github.com/rogerwang/node-webkit/wiki) for details on how to run apps.

Requirements:

* [NodeJS >= 0.10](http://nodejs.org/download/)
* [node-webkit](https://github.com/rogerwang/node-webkit#downloads). Download and extract its contents in `/opt/node-webkit`.


Then:

* Clone repo and run `npm install` or just run `npm install gitw`.
* Install *nw-gyp*: `npm install -g nw-gyp`.
* Rebuild *git-utils* dependency based on the node-webkit version you are running. Eg: `cd node_modules/git-utils && nw-gyp rebuild --target=0.8.6`.
* Run the app! `/opt/node-webkit/nw /path/to/gitw`.

Also, you will find two helper scripts inside the `tools` folder: `build.sh` and `build-new.sh`. Those creates distributable packages for Linux. It asumes you have node-webkit installed on `/opt/node-webkit`.

## Troubleshooting

* [The solution of lacking libudev.so.0](https://github.com/rogerwang/node-webkit/wiki/The-solution-of-lacking-libudev.so.0)
* __ENOSPC error__: You may run into that error when browsing large repositories. You need to increase the maximum number of watches for inotify: `echo fs.inotify.max_user_watches=65536 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p`. That should be enough.
