<!--
SPDX-License-Identifier: CC0-1.0
SPDX-FileCopyrightText: © 2022 Matija Šuklje <matija@suklje.name>
-->

# ChangeLog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) and this project will adhere to [Semantic Versioning](https://semver.org/spec/v2.0.0.html) from its 1.0.0 release onward.


## [Unreleased][]


### Added
- added this `CHANGELOG.markdown`
- improved `README.markdown`, and amongst others, added information regarding Firefox Sync and a tip on how to migrate a huge amount of tabs
- added a method to migrate profiles from the old (pre-0.3) versions of this script
### Changed
- script now automatically changes the Firefox Sync device name to the KDE Plasma Activity name (but it does not really work Firefox itself yet, see [issue #8](https://gitlab.com/hook/activity-aware-firefox/-/issues/8))
- small changes to the help menu
- changed the git tags to start with `v` (as is the convention) and included some annotations
### Removed
### Deprecated
### Fixed
- [Use Activity name (instead of number) as Profile name](https://gitlab.com/hook/activity-aware-firefox/-/issues/7)
### Security


## [0.3][] – 2022-10-01
### Added
- made the script interactive by adding a dialog (including help) on first launch that assists with starting a new Profile or copying some info from either the default or template Profile – started by [@Cristian_Le][], fixed and much improved by [@Sir_Yaro][]
- added a function that removes old profiles – started by [@Cristian_Le][] and polished by [@Sir_Yaro][] (see [README](<./README.markdown#clean-up-of-unused-profiles>))
- added desktop notifications – [@Sir_Yaro][]
- added a depency check for the dialogs and trash – [@Sir_Yaro][]
- added some files to serve as example for a template Profile
- the script nows runs in debug mode if you start it with `activityfirefox +x` – [@Sir_Yaro][]
### Changed
- names of Profiles now include the names of Activities, which makes them much easier to identify
- in general the code is much improved now thanks to [@Cristian_Le][], and [@Sir_Yaro][]
### Deprecated
- **WARNING:** the improvements change the Profile names drastically, so the Profiles created by 0.2 or 0.1 are not used nor automatically cleaned – they are deprecated but remain untouched (see [README](<./README.markdown#clean-up-of-unused-profiles>))
### Fixed
- [Implement a way to clean up Profiles that are not used anymore](https://gitlab.com/hook/activity-aware-firefox/-/issues/3) – **NB** that you still need to manually purge Profiles that were/are not created by `activityfirefox` or were created by an older version (0.1 or 0.2) of it (see [README](<./README.markdown#clean-up-of-unused-profiles>))
- [Use a template when starting a new profile/activity](https://gitlab.com/hook/activity-aware-firefox/-/issues/6)


## [0.2][] – 2021-06-02
### Added
- `README.markdown` to explain what the script does, how to install it, and provide information about known limitations
- `.gitignore`
- a `userChrome.css` that is useful if you use tree-style tabs and want to hide the standard vertical tab bar, but also serves as an example
- made the repository compliant with the [REUSE](https://reuse.software) licensing best practices
### Changed
- cleaned up some code comments and translated them into English
- removed personal and distro bias and made the script work on different setups thanks to [@Cristian_Le][]
### Fixed
- [Installation instruction are not accurate](https://gitlab.com/hook/activity-aware-firefox/-/issues/2)
- [Better detect the right default profile](https://gitlab.com/hook/activity-aware-firefox/-/issues/4)


## [0.1][] – 2021-06-02
### Added
- an initial and very basic version of this the `activityfirefox` wrapper and its `.desktop` file as descbired in [my first blog post about it](https://matija.suklje.name/introducing-activity-aware-firefox), it gets the job done, but makes a lot of assumptions – by [@hook][]


[Unreleased]: https://gitlab.com/hook/activity-aware-firefox/-/compare/v0.3...HEAD
[0.3]: https://gitlab.com/hook/activity-aware-firefox/-/compare/v0.2...v0.3
[0.2]: https://gitlab.com/hook/activity-aware-firefox/-/compare/v0.1...v0.2
[0.1]: https://gitlab.com/hook/activity-aware-firefox/-/tree/v0.1

[@Sir_Yaro]: https://gitlab.com/Sir_Yaro
[@Cristian_Le]: https://gitlab.com/LecrisUT
[@hook]: https://gitlab.com/hook
