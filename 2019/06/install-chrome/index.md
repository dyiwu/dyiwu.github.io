# Install Google Chrome on Linux Mint 19 Tara

<!--more-->
Setup Google Chrome Repository and then install it.

  - Download Google signing key and install it.
```
$ wget -q -O - https://dl.google.com/linux/linux_signing_key.pub \
| sudo apt-key add -
```
  - Set up the Google Chrome repository.
```
$ echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" \
| sudo tee /etc/apt/sources.list.d/google-chrome.list
```
  - Update repository index.
```
$ sudo apt update
```

  - Install Google Chrome Stable version.

```
$ sudo apt install -y google-chrome-stable
```

