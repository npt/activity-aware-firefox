<!--
SPDX-License-Identifier: CC0-1.0
SPDX-FileCopyrightText: © 2021 Matija Šuklje <matija@suklje.name>
-->

# Activity-aware Firefox

[![REUSE status](https://api.reuse.software/badge/gitlab.com/hook/activity-aware-firefox)](https://api.reuse.software/info/gitlab.com/hook/activity-aware-firefox)

## Problem

[Firefox][] is a great and powerful web browser by Mozilla, and [Plasma][] is a great and powerful desktop by KDE.

The problem is that one of the most powerful features of Plasma – Activites – do not work well with Firefox.

There are several pain points in this combination out of the box, including:

- a link opened in one Activity could open in a Firefox window in a different Activity (sometimes even starting that extra Activity, if it was stopped)
- when resuming a stopped Activity with Firefox in it, it would sometimes spawn other Firefox windows that are not related to that Activity
- in general Firefox windows tend to move around Activities where you do not expect them to be

This small script tries to work around those limitations.

[firefox]: https://getfirefox.org
[plasma]: https://kde.org/plasma-desktop/


## Solution: Make Firefox aware of Activites

In short, using `activityfirefox` you can have a Firefox window (or several) in each Plasma Activity, and they will be bound to that Activity.

What `activityfirefox` does is to create a [Firefox Profile][profile] for the Activity (using its unique ID) it was ran in.

[profile]: https://support.mozilla.org/en-US/kb/profiles-where-firefox-stores-user-data

Then every next time you run `activityfirefox` in that Plasma Activity, it will start the Firefox Profile associated with it.


# Installation

Save the `activityfirefox` script to somewhere in your `$PATH`.

In my case, I saved it to `~/.local/bin/activityfirefox` and have that folder in my `$PATH`.

E.g. in [Fish shell][fish], this is what I did:

[fish]: https://fishshell.com/

```shell
git clone https://gitlab.com/hook/activity-aware-firefox.git
mkdir -p ~/.local/bin/
cp activity-aware-firefox/activityfirefox ~/.local/bin/
set -U fish_user_paths ~/.local/bin/ $fish_user_paths
```

Next, make sure your desktop knows about this “new” browser, by giving it a `.desktop` file. For example copy the one provided in this repository:

```shell
mkdir -p ~/.local/share/applications/
cp activity-aware-firefox/activityfirefox.desktop ~/.local/share/applications/
```

And finally, make sure that Plasma uses your new ”Activity-aware Firefox” as the default browser:

- open _System Settings_
- either search for “browser” or navigate manually to: _Personalisation_ ↦ _Applications_ ↦ _Default Applications_
- and there under _Web Browser_ select the new entry called “Activity-aware Firefox”


# Running

From this point onward, just use the “Activity-aware Firefox” browser as your browser of choice and be happy about it :)

## Creating a new Profile

On its absolute first run, `activityfirefox` will create a template profile: `~/.mozilla/firefox/????????.template-profile`

You can copy (or create) settings you want to be present in ever new Activity-aware Firefox Profile into that. An example set-up useful for [Tree-Style Tabs][tst] is present in <./template_example> (see [Hiding the tab row](#hiding-the-tab-row) for what it does).

On any first run within a new Activity, `activityfirefox` will prompt you whether you want to:

- **Template profile copy** – it will copy everything, and only what is in `~/.mozilla/firefox/????????.template-profile`
- **Full copy** – This option will copy _all_ data found in the default Firefox Profile. Please mind it _might_ take a long time and take a lot of disk space. **WARNING:** Please note that Mozilla developers advise against making full profile copy!
- **Minimal** – This is a good trade-off between convenience and resource usage. Only a few important files will be copied over:
	- Bookmarks, Downloads and Browsing History
	- Passwords
	- Site-specific preferences
	- Search engines
	- Personal dictionary
	- Autocomplete history
	- Cookies
	- Security certificate settings
	- Download actions
	- Toolbar customization
	- User preferences
- **New** – This option will create a fresh new profile without copying anything. This is the fastest method, but you will end up with Firefox default settings, so you might need to manually modify settings yourself.

More on this subject: <https://support.mozilla.org/en-US/kb/profiles-where-firefox-stores-user-data>

## Clean-up of unused Profiles

On launch `activityfirefox` will automatically search for any stale Firefox Profiles that it created and which do not have a matching KDE Plasma Activity (anymore, because the Activity was deleted). In that case it will pop up a dialog asking you for confirmation whether you want to delete the unused Profile or not.

**NB:** the script intentionally does not touch any Profiles that were _not_ created by `activityfirefox` or were created by either version 0.2 or 0.1. The best way to clean those out is to manage your existing Profiles through the means of `firefox --ProfileManager`.


# Caveats

There are some caveats, of course:

The biggest annoyance is that due to the fact that [Firefox kills itself if it has been sleeping for a certain period of time][ff_stop] after receiving e.g. `SIGSTOP`, if you resume a stopped Activity, Firefox will start with the default profile. You need to close that window and simply start “Activity-aware Firefox” and it will come back as expected.

A limitation of this script is that add-ons, bookmarks, settings, etc. are _not_ copied (except for `chrome/`) – I suggest using [Firefox Sync][sync] to sync all of this between Firefox Profiles. This is is also needed if you want to move tabs between Profiles/Activities.

Syncing everything through Firefox Sync can take some time when you run the “Activity-aware Firefox” in a new Plasma Activity for the first time – depending on how much stuff you sync, of course.

On the first run `activityfirefox` asks you what you want to copy (if anything) from the default or template Profile (see [Creating a new Profile](#creating-a-new-profile) above).

[sync]: https://www.mozilla.org/en-US/firefox/sync/
[ff_stop]: https://searchfox.org/mozilla-central/source/dom/ipc/ContentParent.h#1490


# Specifics for different use-cases


## How to find your default Firefox Profile

Depending on your installation, your default Firefox Profile will either be in `~/.mozilla/firefox/????????.default-release/` or `~/.mozilla/firefox/????????.default/`.

You can find the exact folder by going through the menu _Help_ ↦ _More Troubleshooting Information_ and in the tab that pops up search for “Profile Directory”.


## Firefox Sync

With Firefox Sync you can decide (per Profile/device) which of the following you want to sync:

- bookmarks
- history
- open tabs – does not auto-sync tabs, but allows you to move tabs between Profiles (and devices)
- logins and passwords – uses Firefox Lockwise
- credit card info
- add-ons
- preferences – not all preferences are synced though, as some are deemed too specific to be safely synced (to work around this you can also use `user.js`)

If you use Firefox Sync, it makes sense to rename each “device”, so you know which Profile is which. A good option is to include the Activity name and the machine name. We are trying to figure out how to automate this step, help is welcome (see [issue #8](https://gitlab.com/hook/activity-aware-firefox/-/issues/8)).


## Rename Firefox windows to match the Activities

In case you want to more easily identify which Firefox window belongs to which Plasma Activity, you can use the [Window Titler add-on][wt] to change the names of Firefox windows.

[wt]: https://addons.mozilla.org/en-GB/firefox/addon/window-titler/

Apparently it should be possible to do this through the Desktop as well using [Native Messaging WebExtension][native_messaging], but that is beyond my skills.

[native_messaging]: https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Native_messaging


## Tree Style Tab


### Hiding the tab row

If you use the [Tree Style Tab add-on][tst] or [Sidebery][sb], you probably want to hide the tab row on the top.

[sb]: https://addons.mozilla.org/firefox/addon/sidebery/
[tst]: https://addons.mozilla.org/firefox/addon/tree-style-tab/

For your convenience, see `template_example/chrome/userChrome.css` in this repository and copy it to your default – or better yet, your template – profile. If the `chrome/` (sub)folder does not exist yet there, create it.

Although the `chrome/` folder is copied when you first run this script, since Firefox 57 user chrome is by default disabled. So even after everything syncs over Firefox Sync, you will need to enable user chrome yourself. An easy way to modify the settings from  `about:config` by storing the settings you want to override into `user.js` and store that in the Profile folder. The included `template_example/user.js` does that for you.

The following command should copy all that is needed to your template profile:

```shell
cp -r template_example/* ~/.mozilla/firefox/????????.template-profile/
```

If the `*.template-profile/` directory does not exist yet, run `activityfirefox` once and either quit or keep the dialog window open while you populate the now created template folder with all that you need.

### Sending groups of tabs to a different Activity

If you want to send whole branches of tabs to a different Firefox Profile (in our case: a Firefox window in a different Plasma Activity), you should head out to `about:addons` ↦ _Tree Style Tab_ ↦ _Preferences_ ↦ _Context Menu_ ↦ _Send Tabs to Other Devices with the context menu via Firefox Sync_ and follow the instructions there.

You will need to do this for every Profile that you want to tab trees to and from, as unfortunately this add-on does not see the device names of Firefox Sync.


## Firefox Multi-Account Containers

If you use the [Multi-Account Containers][containers] add-on, it can sync its containers settings through Firefox Sync, but you need to trigger that through the add-on itself.

[containers]: https://addons.mozilla.org/en-GB/firefox/addon/multi-account-containers/


## Add-ons using local storage

Some more complex add-ons that make use of local storage cannot sync through Firefox Sync. You have to take that into account.


## How to move a vast amount of tabs

If you already have a huge amount of tabs open that you want to migrate to your new Activity-aware Firefox Profiles, I suggest you use the [Bulk URL Opener add-on][bulk].

[bulk]: https://addons.mozilla.org/en-US/firefox/addon/bulkurlopener/


# Inspiration & further reading

This is where I pulled inspiration from to create this script.

- <https://yuenhoe.com/blog/2012/08/associating-firefox-profiles-with-kde-activities/>
- <https://cukic.co/2016/02/08/heavy-activities-setup/>
- <https://bbs.archlinux.org/viewtopic.php?id=137941>
- <https://adrian15sgd.wordpress.com/2012/12/16/soporte-de-actividades-para-firefox/>

I would also like to thank the official `#general:mozilla.org` Mozilla Matrix channel, Ivan Čukić, Kai Uwe Broulik, and Lim Yuen Hoe for all their help. 
