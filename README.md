# Vagrant Middleman Starter Template
Load up a Middleman starter template in Vagrant

## Prerequisites
This project assumes that you have [Vagrant installed](https://www.vagrantup.com/downloads.html).

## Running
1. Fork this repo on Github.
2. Clone _your_ forked repository with `git clone https://github.com/YOURUSERNAME/vagrant-middleman.git`.
3. Run with `vagrant up`.

After Vagrant initializes, Middleman should be running within Vagrant. You can see your Middleman site in http://localhost:4567. If not, ssh into your Vagrant box by running `vagrant ssh` from within your local project folder.

Inside your Vagrant box, check out `~/middleman.log` for any issues.

# Developing with Vagrant
Vagrant rsync allows files from your computer to be synced up with your vagrant box. While Middleman runs within Vagrant, you can edit and add files locally and rsync will essentially copy over the changes to the guest box.

A few tidbits, however:

Because `middleman server` is running within Vagrant, you have to `vagrant ssh` into vagrant to run a `bundle` command.

```bash
  # From within your local project folder
  vagrant ssh

  # Inside your Vagrant box
  cd /srv/<PROJECT NAME>
  bundle install
```

By running `bundle install` from within the `/srv/<PROJECT NAME>` directory within the Vagrant box, this ensures that any updates to the Gemfile or Gemfile.lock will be synced back to your local directory.

See [Vagrantfile](https://github.com/angelocordon/vagrant-middleman/blob/master/Vagrantfile#L15).

# Credits
[Vagrant](http://vagrantup.com/)
[Middleman](https://github.com/middleman/middleman)
[Slate](https://github.com/lord/slate)
