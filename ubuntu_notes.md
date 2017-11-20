#

## Instal Google Chrome browser
Some streaming services, because of proprietary drivers, work only on Google Chrome browser, and not on Firefox or Chromium.

This is for command line installation of the Google Chrome browser (not the open-source Chromium browser)

Source: https://www.linuxbabe.com/ubuntu/install-google-chrome-ubuntu-16-04-lts

```sh
# first open sources.list
sudo nano /etc/apt/sources.list

# then add append this line to the sources.list file
deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main
# ...save and close the file.

# Download Google's signing key...
wget https://dl.google.com/linux/linux_signing_key.pub
# Use apt-key to to add it to your keyring so the package
#   manager can verify the integrity of the Chrome pkg...
sudo apt-key add linux_signing_key.pub

# Update pkg list and install stable version of Chrome...
sudo apt update
sudo apt install google-chrome-stable

# Google Chrome browser linux version ships with built-in
#   Flash player installed under /opt/google/chrome/PepperFlash

```
