# Setup instructions

- Grab a text editor, where you will spend your days and nights
- Install a package manager
- Pimp your Terminal
- Setup git and GitHub
- Install Ruby

## Git

To install `git`, first open a terminal. To open a terminal, you can click on the Ubuntu Start button in the sidebar and type `Terminal`. Then click on the terminal icon.

Then copy this line with `Ctrl` + `C`:

```bash
sudo apt install -y git
```

:bulb: To **paste it in the terminal**, you need to use `Ctrl` + `Shift` + `V`.


## Sublime Text 3 - Your text editor


```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
```


```bash
sudo apt install -y apt-transport-https
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt update
sudo apt install -y sublime-text
```

Sublime Text is free without any time limitation but a popup will appear every ten saves to remind you there is a license to buy. You can hit `Esc` when this happens, but feel free to buy Sublime Text if you really like this one (there are alternatives).


## Oh-my-zsh - Fancy your Terminal

We will use the shell named `zsh` instead of `bash`, the default one.

```bash
sudo apt install -y zsh curl vim imagemagick jq
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
# it will ask for your session password
```

Be careful, those commands will ask you to type your password twice. At the end
your prompt should look like this:

![](images/ubuntu_oh_my_zsh.png)

To make this change stick, restart your laptop (or virtual machine):

```bash
sudo reboot
```


## GitHub

We need to generate SSH keys which are going to be used by GitHub and Heroku
to authenticate you. Think of it as a way to log in, but different from the
well known username/password couple. If you already generated keys
that you already use with other services, you can skip this step.

Open a terminal and type this, replacing the email with **yours** (the
same one you used to create your GitHub account). It will prompt
for information. Just press enter until it asks for a **passphrase**.

```bash
mkdir -p ~/.ssh && ssh-keygen -t ed25519 -o -a 100 -f ~/.ssh/id_ed25519 -C "TYPE_YOUR_EMAIL@HERE.com"
```

**NB:** when asked for a passphrase, put something you want (and that you'll remember),
it's a password to protect your private key stored on your hard drive. You'll type,
nothing will show up on the screen, **that's normal**. Just type the passphrase,
and when you're done, press `Enter`.

Then you need to give your **public** key to GitHub. Run:

```bash
cat ~/.ssh/id_ed25519.pub
```

It will prompt on the screen the content of the `id_ed25519.pub` file.


>\- Copy that text from `ssh` to the end of your email address  
>\- Go to [github.com/settings/ssh](https://github.com/settings/ssh)  
>\- Click on `Add SSH key`  
>\- Fill in the Title with your computer name  
>\- Paste the **Key**  
>\- Finish by clicking on the **Add key** green button.


To check that this step is completed, in the terminal run this.

```bash
ssh -T git@github.com
```

:warning: You will be prompted a warning, type `yes` then `Enter`.

This is the expected result:

```
# Hi --------! You've successfully authenticated, but GitHub does not provide shell access
```

&nbsp;

&nbsp;&nbsp;&nbsp; :white_check_mark: If you got this message, the keys were added successfully!

&nbsp;&nbsp;&nbsp; :x: If you encountered an error, you will have to try again. 


---

#### :wrench: Potential Fixes

<details>
  <summary>If `ssh -T git@github.com` does not work</summary>

  &nbsp;
  
  
  Try running this command before trying again:

  ```bash
  ssh-add ~/.ssh/id_ed25519
  ```
  </details>

---


Don't be in a rush, take time to [read this article](http://sebastien.saunier.me/blog/2015/05/10/github-public-key-authentication.html) to get a better
understanding of what those keys are used for.


## Dotfiles (Standard configuration)

Based on [LeWagon's dotfiles](https://www.github.com/lewagon/dotfiles)

Hackers love to refine and polish their shell and tools. As your configuration is personal, you need your own repository storing it, so you first need to fork it to your GitHub account.

You should arrive on a page that looks like this. Make sure to **select your GitHub account**.


```bash
export GITHUB_USERNAME=replace_this_with_your_github_username

# Example:
#   export GITHUB_USERNAME=ssaunier
```

Now copy/paste this very long line in your terminal. Do **not** change this one.

```bash
mkdir -p ~/code/$GITHUB_USERNAME && cd $_ && git clone git@github.com:$GITHUB_USERNAME/dotfiles.git
```

Run the `dotfiles` installer.

```bash
cd ~/code/$GITHUB_USERNAME/dotfiles
zsh install.sh
```

Then run the git installer:

```bash
cd ~/code/$GITHUB_USERNAME/dotfiles
zsh git_setup.sh
```

:point_up: This will **prompt** you for your name (`Firstname Lastname`) and your email.

Be careful, you **need** to put the **same** email as the one you sign up with on GitHub.

Please now **quit** all your opened terminal windows.

### Sublime Text auto-configuration

Open a new terminal and type this:

```bash
cd ~/code
stt
```

It will **open Sublime Text in the context of your current folder**. That's how we'll use it.

**Close Sublime text** and open it again:

```bash
stt
```

**Wait 1 minute** for additional packages to be automatically installed (New tabs with text will automatically open, containing documentation for each new package installed). TO follow package installation, you can go to `View > Show console`.

To check if plugins are installed, open the Command Palette (`⌘` + `⇧` + `P` on OSX, `Ctrl` + `⇧` + `P` on Linux), type in `Packlist` and then `Enter`, you should see a couple of packages installed (like [Emmet](http://emmet.io/)).

If you don't, please install all of them manually. The list is referenced [here](https://github.com/lewagon/dotfiles/blob/master/Package%20Control.sublime-settings).

When it's done, you can close Sublime Text.


## Installing Ruby (with [rbenv](https://github.com/sstephenson/rbenv))

First, we need to clean up any previous Ruby installation you might have:

```bash
rvm implode && sudo rm -rf ~/.rvm
# If you got "zsh: command not found: rvm", carry on. It means `rvm` is not
# on your computer, that's what we want!

rm -rf ~/.rbenv
```

Then in the terminal, run:

```bash
sudo apt install -y build-essential tklib zlib1g-dev libssl-dev libffi-dev libxml2 libxml2-dev libxslt1-dev libreadline-dev
```
```bash
sudo apt clean
```
```bash
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
```
```bash
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
```

**Close your terminal and open it again** (Alt+F4 and restart it). If you get a warning, just **ignore** it from now (Ruby is not installed yet).


Now, you are ready to install the latest ruby version and set it as the default version.

Run this command, it will **take a while (5-10 minutes)**

```bash
rbenv install 2.6.6
```

Once the ruby installation is done, run this command to tell the system
to use the 2.6.6 version by default.

```bash
rbenv global 2.6.6
```

Then **restart** your Terminal again (close it and reopen it).

```bash
ruby -v
```

You should see something starting with `ruby 2.6.6p`.

## Installing some gems


```bash
gem install rake bundler rspec rubocop rubocop-performance pry pry-byebug hub colored http
```

If you encounter the following error:

`
ERROR: While executing gem ... (TypeError)
incompatible marshal file format (can't be read)
format version 4.8 required; 60.33 given
`

Run the following command:
```bash
rm -rf ~/.gemrc
```

Rerun the command to install the gems.

**Never** install a gem with `sudo gem install`! Even if you stumble upon a Stackoverflow answer
(or the Terminal) telling you to do so.


## Installing Node (with [nvm](https://github.com/nvm-sh/nvm))

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | zsh
```

Restart your terminal and run the following:

```bash
nvm -v
```
You should see a version. 

Now let's install node:

```bash
nvm install 14.15.0
```

When the command returns, run

```bash
node -v
```



## PostgreSQL


```
sudo apt install -y postgresql postgresql-contrib libpq-dev build-essential
sudo -u postgres psql --command "CREATE ROLE `whoami` LOGIN createdb;"
```


## Ubuntu inotify

Ubuntu is always tracking changes in your folders and to do this it uses inotify.
By default the Ubuntu limit is set to 8192 files monitored.

As programming involves a lot of files, we need to raise this limit.
In your terminal run:

```bash
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```


## Extra


### Install useful terminal tools

`tree` is a nice tool to visualize a directory tree inside the terminal:

`ncdu` is a text-based interface disk utility.

`htop` is an interactive process viewer.

`tig` is a text-mode interface for `git`.

`hub` is a command line tool that wraps `git` in order to extend it with extra features and commands that make working with GitHub easier.

```bash
sudo apt install tree ncdu htop tig hub
```
