# VRChat String Downloader Override
A program used to override the result of VRChat's String Downloader function. This is fairly simple if you know any web development. It allows for exploiting a vulnerability that most VRChat world creator don't seem to know about or care about.

To the best of my knowlage, this doesn't break any of VRChat's rules since it doesn't affect the VRChat client in anyway. All this program does is make your computer redirect VRChat's web request to `localhost` and then pretent to be the intended webserver. This can be done in a lot of other ways, such as with a http-proxy or some special router settings, but this is the method I knew the most about.

The program does request Admin privileges to run, because the programs alters your computer's `hosts` file and has to run a HTTP listener with self-signed SSL certificates. See the Uninstall instrustions below for details on how to get rid of the created certificates and `hosts` file enties.

If you don't trust me and can't read&complie the code yourself, just leave and don't worry about this. This program will hopefully stop being useful once VRChat world creators learn to fix this vulnerability in their worlds.

If you are a world creator worried about this program or before mentioned vulnerability, check the Mitigation section below.

## Requirements
* .Net Framework 6.0 (default on most Windows computers)
* Admin privileges

## How to use
* Start **VRChat**
* Enter the desired world
* Check the log for `[String Downloader]` log entries containing URLs
* Copy the URLs from the log entries
* Start the **VRChat String Downloader Override** program
* Repeat this for each URL you found
  * Click the menu strip option: *Pages* -> *Add Page*
  * Paste the URLs into the *Overridden URL* field
  * Click the *Get String* button to download the string
  * Edit the downloaded string to your liking
* Click the *Start* button
* Return to **VRChat**
* Rejoin the desired world
* Check the log list in the **VRChat String Downloader Override** program to see if it received the webrequests
* Check if the world was affacted by your changed string and have fun

The created pages can be saved and loaded for later re-use.
* Click the menu strip option: *Pages* -> *Save Pages*
  * This will allow you to save the pages to a file
* Click the menu strip option: *Pages* -> *Load Pages*
  * This will allow you to load pages from a file

If the String Downloader function uses URLs that aren't part of VRChat's list of trusted URL, then a custom SSL certificate will need to be made to include those hostnames.
* Create the pages and enter the desired URLs on them
* Click the menu strip option: *Certificates* -> *Create new Custom SSL*

## Example world
I made an example world where you can test this vulnerability.
* WIP

## Uninstall
In case your want to delete everything this program has done or made on your computer.

### `hosts` file entries
The program should clean up this on its own whenever you start it or close it. But in case of improper shutdown some changes could remain and cause issues.
* Start **Notepad** as administrator
* Open the `C:\Windows\System32\drivers\etc\hosts` file
* Check for any entries that end in "# VrcStringDownloaderOverride" and delete them
* Save the file

### `netsh http` setting
Having this remain on your computer shouldn't cause any issues, unless you happen to run a webserver locally on your computer.
* Start **CMD** as administrator
* Use the `netsh http delete sslcert ipport=0.0.0.0:443` command to delete the installed certificate

### Certificates
Having the certificates remain on your computer after you are done using the program should cause you no issues. They are self-signed by your computer and no one else has them or uses them. You can delete them without issue, but deleting other certificates from your computer could cause issues for you.
* The self-signed CA certificate with subject "CN = VRChat String Downloader Override CA" is stored in the `Local Computer\Trusted Root Certifiation Authorities\Certificates` folder
* The SSL certificates with subject "CN = VRChat String Downloader Override SSL" are stored in the `Local Computer\Personal\Certificates` folder

## Vulnerability
I might call this a *vulnerability*, but mostly it is just an oversight on the side of VRChat world creators. If you have a world that uses the String Downloader function to check if a player should have access to some exclusives or have spacial privileges, you might have inadvertently made this vulnerability in your world.

A simple example:
* You join a world and the world uses the String Downloader function to get a list of privileged players from `pastebin.com`
  * But what happens if you can't access `pastebin.com`?
  * What happens if `pastebin.com` returns something wrong?
  * What happens if you make it so something you wrote is returned instead of whatever was on `pastebin.com`?
* A world creator should not trust that you get the list they have posted on their private little `pastebin.com`
* What the **VRChat String Downloader Override** program does is simply replace the downloaded string from `pastebin.com` with whatever you want it to be

## Mitigation
First of all, don't have important information be downloaded using the String Downloader function. If you have a list of players with admin/moderator privileges in your world, you should hardcoded in your your world. If the list of privileged players is hardcoded, no one can see it or edit it without literally breaking VRChat rules. Hopefully the list of players that need these super special privileges doesn't change too often.

For lists of players that might change more often than you want to re-upload your world, such as for patrons, you might want to use the String Downloader function. But if you want this to be secure, you will need at least a minimum of security. In network security, the simplest way to ensure that the source of a message is from who you think it is from, is with a [Message authentication code (MAC)](https://en.wikipedia.org/wiki/Message_authentication_code). wikipedia explains it nicely, so I won't go into too much detail, but simply put it works like this:
* WIP
