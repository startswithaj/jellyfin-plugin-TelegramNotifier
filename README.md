## About this fork

This is a fork of [RomainPierre7/jellyfin-plugin-TelegramNotifier](https://github.com/RomainPierre7/jellyfin-plugin-TelegramNotifier). All credit for the plugin goes to its original author. The rest of this README is upstream's.

### What this fork adds

- **Item-added notifications are no longer delayed by ~20 minutes.** Upstream waits until an item has provider ids (TVDB/TMDB/IMDb), and only gives up after 10 retries, roughly 20 minutes. Brand-new episodes and home videos often never get provider ids, so they always waited the full time. This fork sends the notification as soon as Jellyfin's first metadata refresh for the item has finished.
- **Items without artwork are no longer dropped.** If the poster can't be fetched (a newly created season, for example), upstream drops the whole notification and logs a misleading "please check your configuration" error. This fork sends it as a text message instead.
- **A deleted item no longer stalls the notification queue** for the rest of that processing run.
- **The bot token is no longer written to the Jellyfin log** when a message fails to send ([upstream #78](https://github.com/RomainPierre7/jellyfin-plugin-TelegramNotifier/issues/78)).
- **Releases are built by GitHub Actions**, not on a local machine, and come with signed build provenance. From 12.3.0.1 onward you can verify that a downloaded `TelegramNotifier.zip` was built by this repository's workflow:

  ```
  gh attestation verify TelegramNotifier.zip -R startswithaj/jellyfin-plugin-TelegramNotifier
  ```

### Installing from this fork

Follow the install steps below, but use this repository URL instead of the upstream one:

```
https://raw.githubusercontent.com/startswithaj/jellyfin-plugin-TelegramNotifier/main/manifest.json
```

If you already have the upstream repository added, disable it. Both repositories serve the same plugin, and Jellyfin's auto-update installs the highest version from any enabled repository, so a newer upstream release would replace this fork's build.

---

# Jellyfin Plugin Telegram Notifier

<a href='https://ko-fi.com/B0B8112Y0Y' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://storage.ko-fi.com/cdn/kofi1.png?v=3' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>

![GitHub Downloads (all assets, all releases)](https://img.shields.io/github/downloads/RomainPierre7/jellyfin-plugin-TelegramNotifier/total)

![GitHub Downloads (all assets, latest release)](https://img.shields.io/github/downloads/RomainPierre7/jellyfin-plugin-TelegramNotifier/latest/total)

This [Jellyfin](https://github.com/jellyfin) plugin provides notification functionalities via Telegram for various events occurring within your Jellyfin server. Stay informed about playback activities, user management, plugin operations, and more right from your Telegram account. You can personalize the configuration for each user.

<p align="center">
<img src="assets/logo.png" alt="Logo" width="300" height="300">

🧰 A troubleshooting guide is available [here](https://github.com/RomainPierre7/jellyfin-plugin-TelegramNotifier/blob/main/docs/Troubleshooting.md).

> [!IMPORTANT]
> If you encounter a problem, a bug or have an idea for a new feature, submit an issue [here](https://github.com/RomainPierre7/jellyfin-plugin-TelegramNotifier/issues). Please, submit only one feature or bug per issue.

> [!TIP]
> If you're finding value in this project and it's been helpful to you, consider giving it a star ⭐️ on GitHub ! Your support means a lot and helps others discover the project too.

## Table of contents

- [Install the plugin](#install-the-plugin)
- [Use the plugin](#use-the-plugin)
- [Install the project (for developers)](#install-the-project-for-developers)

## Install the plugin

To install the plugin on your Jellyfin server, you need to follow these steps:

1. Go to the Jellyfin dashboard
2. Go to the **Plugins** section
3. Click on the **Manage Repositories** button
4. Click on **New Repository**
5. Add the depot URL: 

Repository Name: ```Telegram Notifier```

Repository URL:
```
https://raw.githubusercontent.com/RomainPierre7/jellyfin-plugin-TelegramNotifier/main/manifest.json
```

6. Go back to the **plugins** page
7. Search for the plugin: ```Telegram Notifier```

> Make sure to enable the **All** or **Available** filter

<img src="assets/banner.png" alt="Logo" width="267" height="150">

8. Click on the **Install** button
9. Restart the Jellyfin server

> **Note:** You can also install the plugin manually by downloading the latest release from the [releases page](https://github.com/RomainPierre7/jellyfin-plugin-TelegramNotifier/releases).

## Use the plugin

To use the plugin, you need to follow these steps:

1. Go to the Jellyfin dashboard
2. Go to the **Plugins** section
3. Click on the **Telegram Notifier** plugin
4. Click on **settings**
5. Configure the plugin

### Configuration

1. Select the user you want to configure
2. Enter the Telegram bot token
> **Note:** You can create a Telegram bot by sending the command ```/newbot``` to the [BotFather](https://t.me/botfather) and collect the token.
3. Enter the chat ID
> **Note:** You can get the collect the chat ID by sending a message to the bot and then go to the URL: ```https://api.telegram.org/bot<YourBOTToken>/getUpdates```
4. Click on the **Test** button to test the configuration
5. Enable the notifications for the user
6. Select the events you want to be notified about

Available events:
- Item added
- Item deleted
- Playback start
- Playback progress
- Playback stop
- Subtitle download failure *(soon...)*
- Authentication failure *(soon...)*
- Authentication success *(soon...)*
- Session start
- Pending restart *(soon...)*
- Task completed *(soon...)*
- Plugin installation cancelled
- Plugin installation failed
- Plugin installed
- Plugin installing
- Plugin uninstalled
- Plugin updated
- User created
- User deleted
- User locked out
- User password changed
- User updated *(soon...)*
- User data saved *(soon...)*

7. Click on the **Save** button

**Example of the configuration page:**

![Configuration page exemple](assets/config.png)

## Install the project (for developers)

To install the project, you need to follow these steps:

> **Note:** You need to have the [.NET 9.0 SDK](https://dotnet.microsoft.com/download) installed on your machine.

1. Clone the repository
2. Install the dependencies
3. Compile the plugin
4. Install the plugin

### 1. Clone the repository

```bash
git clone https://github.com/RomainPierre7/jellyfin_telegram_notifier.git
```

### 2. Install the dependencies

```bash
dotnet add package Jellyfin.Controller
```

### 3. Compile the plugin

```bash
dotnet build
```

or use the **Makefile**:

This command will only build the dll file in the ```plugin``` directory.

```bash
make dev
```

Other commands are available in the **Makefile**:

- ```make build``` - Build the project
- ```make publish``` - Publish the project
- ```make clean``` - Clean the project
- ```make dev``` - Build the project and copy the `dll` file in the ```plugin``` directory
- ```make plugin``` - Publish the project and copy the `dll` file in the ```plugin``` directory

### 4. Install the plugin

To install the plugin, you have to find the Jellyfin plugin directory. It depends on your installation but the most common paths are:

- **Linux**: ```/var/lib/jellyfin/plugins```
- **Windows**: ```C:\ProgramData\Jellyfin\Server\plugins```
- **Docker**: ```/config/plugins``` or ```/config/data/plugins```

Then, you have to copy the ```Jellyfin.Plugin.TelegramNotifier.dll``` file in a folder in the plugin directory.

Example:

```
Plugin
│
└── TelegramNotifier
    │
    └── Jellyfin.Plugin.TelegramNotifier.dll
```

Finally, you have to restart the Jellyfin server.

### 5. Tips

If you want to easily test the plugin, you can use the following command to download a video file directly on your server:

```bash
curl https://cdn.pixabay.com/video/2025/03/28/268290_large.mp4 --output titanic.mp4
```
