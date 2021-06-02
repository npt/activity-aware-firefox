# Activity-aware Firefox


## Problem

[Firefox][] is a great and powerful web browser by Mozilla, and [Plasma][] is a great and powerful desktop by KDE.

The problem is that one of the most powerful features of Plasma – Activites – do not work well with Firefox.

There are several pain points in this combination out of the box, including:

- a link opened in one Activity could open in a Firefox window in a different Activity (sometimes even starting that extra Activity, if it was stopped)
- when resuming a stopped Activity with Firefox in it, it would sometimes spawn other Firefox windows that are not related to that Activity
- in general Firefox windows tend to move around Activities where you do not expect them to be

This 

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

```fish
git clone https://gitlab.com/hook/activity-aware-firefox.git
mkdir -p ~/.local/bin/
cp activity-aware-firefox/activityfirefox ~/.local/bin/
set -U fish_user_paths ~/.local/bin/ $fish_user_paths
```

Next, make sure your desktop knows about this “new” browser, by giving it a `.desktop` file. For example copy the one provided in this repository:

```fish
mkdir -p ~/.local/share/applications/
cp activity-aware-firefox/activityfirefox.desktop ~/.local/share/applications/
```

And finally, make sure that Plasma uses your new ”Activity-aware Firefox” as the default browser:

- open _System Settings_
- either search for “browser” or navigate manually to: _Personalisation_ ↦ _Applications_ ↦ _Default _Applications_
- and there under _Web Browser_ select the new entry called “Activity-aware Firefox”


# Running

From that point onward, just use the “Activity-aware Firefox” browser as your broser of choice and be happy about it :)


# Caveats

There are some caveats, of course:

The biggest annoyance is that due to the fact that [Firefox kills itself if it has been sleeping for a certain period of time][ff_stop] after receiving e.g. `SIGSTOP`, if you resume a stopped Activity, Firefox will start with the default profile. You need to close that window and simply start “Activity-aware Firefox” and it will come back as expected.


A limitation of this script is that add-ons, bookmarks, settings, etc. are _not_ copied (except for `chrome/`) – I suggest using [Firefox Sync][sync] to sync all of this between Firefox Profiles. This is in order

As a consequence, syncing everything through Firefox Sync can take some time when you run the “Activity-aware Firefox” in a new Plasma Activity for the first time.

[sync]: https://www.mozilla.org/en-US/firefox/sync/
[ff_stop]: https://searchfox.org/mozilla-central/source/dom/ipc/ContentParent.h#1490

# Specifics for different use-cases


## How to find your default Firefox Profile

Depending on your installation, your default Firefox Profile will either be in `~/.mozilla/firefox/????????.default-release/` or `~/.mozilla/firefox/????????.default/`.

You can find the exact folder by going through the menu _Help_ ↦ _More Troubleshooting Information_ and in the tab that pops up search for “Profile Directory”.


## How to clean up Firefox Profiles not used anymore

After you stop using some Plasma Activities and as such the Firefox Profiles, you may be left with Profiles staying around. You can use `firefox --ProfileManager` to manage your existing Profiles, including removing them.

If I find a safe way to clean up in an automated way, I will add it.


## Rename Firefox windows to match the Activities

If you want to more easily identify which Firefox window belongs to which Plasma Activity, you can use the [Window Titler add-on][wt] to change the names of Firefox windows.

[wt]: https://addons.mozilla.org/en-GB/firefox/addon/window-titler/


## Tree Style Tab


### Hiding the tab row

If you use the [Tree Style Tab add-on][tst], you probably want to hide the tab row on the top.

[tst]: https://addons.mozilla.org/en-US/firefox/addon/tree-style-tab/

For your convenience, see `userChrome.css` in this repository and copy it to your default profile. If `chrome/` folder does not exist yet in your default profile, create it.

Although the `chrome/` folder is copied when you first run this script, since Firefox 57 user chrome is by default disabled. So even after everything syncs over Firefox Sync, you will need to enable user chrome yourself. You can do so, if you:

- open `about:config`
- confirm that you will be careful ;)
- search for `toolkit.legacyUserProfileCustomizations.stylesheets` and enable it
- restart Firefox, so that change applies


### Sending groups of tabs to a different Activity

If you want to send whole branches of tabs to a different Firefox Profile (in our case: a Firefox window in a different Plasma Activity), you should head out to `about:addons` #' ↦ ↦ _Tree Style Tab_ ↦ _Preferences_ ↦ _Context Menu_ ↦ _Send Tabs to Other Devices with the context menu via Firefox Sync_ and follow the instructions there.


## Firefox Multi-Account Containers

If you use the [Multi-Account Containers][containers] add-on, it can sync its containers settings through Firefox Sync, but you need to trigger that through the add-on itself.

[containers]: https://addons.mozilla.org/en-GB/firefox/addon/multi-account-containers/


## Worldbrain’s Memex and other add-ons using local storage

Some more complex add-ons that make use of local storage – e.g. [Worldbrain’s Memex][memex] – cannot sync through Firefox Sync. You have to take that into account.

(At least for Memex, its team is working on a way to sync between devices separately.)

[memex]: https://getmemex.com/


# Previous work & further reading

