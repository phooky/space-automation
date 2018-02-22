The service that orchestrates all of the "home automation" tasks in the space is Home Assistant.  Any lights, sensors, switches, etc., talk to it, and it provides a web and app front end for viewing and setting the state of all the things.
To view the front end visit `automation:8123` in your browser while you are connected to the space wifi.  This is system is not on the internet, so you will not be able to access it remotely without setting up port forwarding on golden.

If that didn't work, maybe homeassistant isn't running?  To restart it, log onto `automation` (container on golden) as user `automator` and run:

```bash
$ .restart_hass.sh
```

The configuration files for Home Assistant live in `/home/holly/space-automation/homeassistant/` and are symlinked to the files in `/etc/space-automation/homeassistant` where they can be accessed for editing by anyone in the group `automation`.  If you would like to edit the configs, ask to be added to the group by an administrator (George, Holly, etc.).

The directory is under version control, don't forget to check in and push your changes.

Home Assistant was installed by `pip3` and its non-user-editable files live in `/usr/lib/python3.6/site-packages/homeassistant`.
# Configuration

## Quick Tips

If you plan to edit the config files in vi, add this to your `.exrc` or `.virc` or `.vimrc` or whatever the kids are using these days:

```vim
" keep the computer safe from being thrown across the room while writing yaml
autocmd FileType yaml set tabstop=2|set shiftwidth=2|set expandtab
```

## Front End

### How do I make a tab?

A tab is called a `view`.  So in one of the group files make a group for what you want in the tab and set view to yes:

```yaml
lights:
  view: yes
  name: Ligts
  entities:
    - group: main_room_card
    - group: back_room_card
```

The main page of the Home Assistant app and webview is called the `default_view`.  If you want to customize it:

```yaml
default_view:
  view: yes
  entities:
    - group: lights_card
    - group: motion_sensors
    - media_player.spotify
```

### How do I make a card on a view?

In the `groups.yaml`, or in `groups/cards.yaml`

```yaml
lights_card:
  name: Lights
  entities:
    - group.main_room
    - group.back_room
```

And then reference that card group in the default_view group, or whatever group that represents the tab that you want it on.


# Useful commandline tools

If you want to check that your config file is valid without restarting the service (because yaml):

```bash
$ hass --script check_config -c /etc/space-automation/homeassistant
```

You can get configuration data from the lights with [scenegen](https://github.com/home-assistant/scenegen). So you can set up all your lights exactly how you want them for a scene, then run scenegen and use the output to fill in your scene configuration file.  However, this will not give you `rgb_color` data, only `color_temp` and `brightness`, so you'll need to put any colors in by hand.

```bash
$ python3 scenegen.py http://10.0.[redacted]
```
**If you include the slash at the end of the url it won't work.**

# Components

If you are working on the system and you need to know how to access components and services, check the documentation folder.


