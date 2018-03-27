---
layout: post
title: "Common Issues"
permalink: commonissues
category: user-guide
github: "https://github.com/ember-cli/ember-cli.github.io/blob/master/_posts/2013-04-03-common-issues.md"
---

### `npm` Package Management with `sudo`

Installing packages such as `bower` with `sudo` powers can lead to permissions
issues and ultimately to problems installing dependencies. See
[https://gist.github.com/isaacs/579814](https://gist.github.com/isaacs/579814)
for a collection of various solutions.

### Installing From Behind a Proxy

If you're behind a proxy, you might not be able to install because Ember CLI–or
some of its dependencies–tries to `git clone` a `git://` URL. (In this scenario,
only `http://` URLs will work).

You'll probably get an error like this:

{% highlight bash %}
npm ERR! git clone git://github.com/jgable/esprima.git Cloning into bare repository '/home/<username>/.npm/_git-remotes/git-github-com-jgable-esprima-git-d221af32'...
npm ERR! git clone git://github.com/jgable/esprima.git
npm ERR! git clone git://github.com/jgable/esprima.git fatal: unable to connect to github.com:
npm ERR! git clone git://github.com/jgable/esprima.git github.com[0: 192.30.252.129]: errno=Connection timed out
npm ERR! Error: Command failed: fatal: unable to connect to github.com:
npm ERR! github.com[0: 192.30.252.129]: errno=Connection timed out
{% endhighlight %}

As a workaround you can configure `git` to make the translation:

{% highlight bash %}
git config --global url."https://".insteadOf git://
{% endhighlight %}

### Using Canary Build instead of release

For Ember: `bower install ember#canary --resolution canary`
For `ember-data`: `npm install --save-dev emberjs/data#master`

### Windows Build Performance Issues

See [The Windows Section](#windows) for more details.

### PhantomJS on Windows

When running tests on Windows via PhantomJS the following error can occur:

{% highlight bash %}
events.js:72
throw er; // Unhandled 'error' event
^
Error: spawn ENOENT
at errnoException (child_process.js:988:11)
at Process.ChildProcess._handle.onexit (child_process.js:779:34)
{% endhighlight %}

In order to fix this ensure the following is added to your `PATH`:

`C:\Users\USER_NAME\AppData\Roaming\npm\node_modules\phantomjs\lib\phantom`

### Cygwin on Windows

Node.js on Cygwin is no longer supported [more
details](https://github.com/nodejs/node/wiki/Installation#building-on-cygwin)
Rather then using Cygwin, we recommend running Ember CLI natively on windows,
or via the new [Windows Subsystem
Linux](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).

### Usage with Docker

When building your own [Docker](http://docker.com) image to build Ember
applications and run tests, there are a couple of pitfalls to avoid.

* PhantomJS requires `bzip2` and `fontconfig` to already be installed.
* After installing PhantomJS, you will need to manually link PhantomJS to
  `/usr/local/bin` if that is not done by the install process.
* Testem uses the `which` command to locate PhantomJS, so you must install
  `which` if it is not included in your base OS.

### Usage with Vagrant

[Vagrant](http://vagrantup.com) is a system for automatically creating and
setting up development environments that run in a virtual machine (VM).

Running your Ember CLI development environment from inside of a Vagrant VM will
require some additional configuration and will carry a few caveats.

#### Ports

In order to access your Ember CLI application from your desktop's web browser,
you'll have to open some forwarded ports into your VM. Ember CLI by default
uses two ports.

* For serving assets the default is `4200`. Can be configured via `--port 4200`.
* For live reload there is no default. Can be configured via `---live-reload-port=9999`.

To make Vagrant development seamless these ports will need to be forwarded.

{% highlight ruby %}
Vagrant.configure("2") do |config|
  # ...
  config.vm.network "forwarded_port", guest: 4200, host: 4200
  config.vm.network "forwarded_port", guest: 9999, host: 9999
end
{% endhighlight %}

#### Watched Files

The way Vagrant syncs directories between your desktop and vm may prevent file
watching from working correctly. This will prevent rebuilds and live reloads
from working correctly. There are several work arounds:

1. Watch for changes by polling the file system via: `ember serve --watcher polling`.
2. Use [nfs for synced folders](https://docs.vagrantup.com/v2/synced-folders/nfs.html).

#### VM Setup

When setting up your VM, install Ember CLI dependencies as you normally would.
Some of these dependencies (such as [broccoli-sass](#sass)) may have native
depenencies that may require recompilation. To do so run:

```
npm rebuild
```

#### Provider

The two most common Vagrant providers, VirtualBox and VMware Fusion, will both
work. However, VMware Fusion is substantially faster and will use less battery
life if you're on a laptop. As of now, VirtualBox will use 100% of a single CPU
core to poll for file system changes inside of the VM.
