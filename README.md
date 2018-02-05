## EmberCLI's official website <a href="https://ember-community-slackin.herokuapp.com" target="_blank"><img src="https://ember-community-slackin.herokuapp.com/badge.svg"></a>

Install Jekyll and a special gem provided by GitHub:

```sh
$ gem install bundler
$ bundle
```

Open the directory which contains the repo and run Jekyll:

```sh
$ bundle exec jekyll serve -w
```

You can now view the result at [http://localhost:4000](http://localhost:4000).

## Windows Users, read on

If you want to help with the development of this site and you're using Windows,
please read [this guide](http://jekyll-windows.juthilo.com) about how to run
Jekyll on your OS.

Alternatively, use bash via Windows Subsystem for Linux (WSL):

- Install/setup [WSL][wsl-install]
- Install ruby, perhaps use [rbenv] or [linuxbrew.sh]

[wsl-install]: https://msdn.microsoft.com/en-us/commandline/wsl/install-win10
[rbenv]: https://github.com/rbenv/rbenv#installation
[linuxbrew.sh]: http://linuxbrew.sh
