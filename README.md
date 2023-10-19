# VRChat String Downloader Override
A program used to override the result of VRChat's String Downloader function. This is fairly simple if you know any web development.

To the best of my knowlage, this doesn't break any of VRChat's rules since it doesn't affect the VRChat client in anyway. All this program does is make your computer redirect VRChat's web request to `localhost` and then pretent to be the intended webserver. This can be done in a lot of other ways, such as with web-proxies or some special router settings, but this is the method I knew the most about.

The program does request Admin privileges to run, because the programs alters your computer's `host` file and has to run a HTTP listener with custom SSL certificates. See the Uninstall instrustions below for details on how to get rid of the created certificates and `host` file enties.

If you don't trust me and can't read&complie the code yourself, just leave and don't worry about this. This program will hopefully stop being useful once VRChat world creators learn to fix this issue in their worlds.

If you are a world creator worried about this program, check the Mitigation section below.

## Requirements
* .Net Framework 6.0 (default on most Windows computers)
* Admin privileges

## How to use
### Getting information
* Start **VRChat**
* Enter the desired world
* Check the log for `[String Downloader]` log entries containing URLs
* Copy the URLs from the log entries
* Start the **VRChat String Downloader Override** program
* Click the menu strip option: *Pages* -> *Add Page* for each URL you found
* Paste the URLs into the *Overridden URL* field of each *Page* tap
* Click the *Get String* button to download the string
* Editing the downloaded string to your liking
* Click the *Start* button
* Return to **VRChat**
* Rejoin the desired world
* Ensure the log list in the **VRChat String Downloader Override** program correctly received the webrequests
* Check if the world was affacted by your changed string and have fun

## Uninstall
In case your want to delete everything this program has done or made on your computer.

### `hosts` file entries
The program should clean up this on its own whenever you start it or close it. But in case of improper shutdown some changes could remain and cause issues.
* Start **Notepad** as administrator
* Open the `C:\Windows\System32\drivers\etc\hosts` file
* Check for any entries that end in "# VrcStringDownloaderOverride" and delete them
* Save the file

### `netsh http * sslcert`
Having this remain on your computer shouldn't cause any issues, unless you happen to run a webserver locally on your computer.
* Start **CMD** as administrator
* Use the `netsh http delete sslcert ipport=0.0.0.0:443` to delete the installed certificate

### Certificates
Having the certificates remain on your computer after you are done using the program should cause you no issues. They are self-signed by your computer and no one else has them or uses them. You can delete them without issue, but deleting other certificates from your computer could cause issues for you.
* The self-signed CA certificate with subject "CN = VRChat String Downloader Override CA" is stored in the "Local Computer\Trusted Root Certifiation Authorities\Certificates" folder
* The SSL certificates with subject "CN = VRChat String Downloader Override SSL" are stored in the "Local Computer\Personal\Certificates" folder

## Mitigation
In network security, the simplest way to ensure that the source of a message is from who you think it is from, is with a [Message authentication code (MAC)](https://en.wikipedia.org/wiki/Message_authentication_code). wikipedia explains it nicely, so I won't go into too much detail, but simply put it works like this:
* WIP
