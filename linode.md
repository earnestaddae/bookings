### How to host on linode

- Create or sign in to account
- Create linode (the name of their server)
- Choose your package by selecting OS and CPU (default shared minimum 5USD)
- Create your root password
- Click on `create linode`
- Get your provision
- Copy your IP address
- Go to terminal and type `ssh root@IPAddress[copied]` and confirm
- You are in your server at address `root@localhost`
- Run `apt update` supposed you selected a linux OS to update your server
- Run `apt upgrade` to upgrade your packages or software

### After the upgrade

- Install the tools you need to host your application
- DB postgres run `apt search postgres` and look for your version of postgreSQL
- Run `apt install postgresql-version`, in my case I installed `postgresql-12`
- After installation run `service postgresql status` to check if it's running

### Installing your server - Caddyserver

- Go to [Debian, Ubuntu, Raspbian](https://caddyserver.com/docs/install) to see installation for Ubuntu OS
- Copy and paste each line of the installation guide
- Once that is done, you now have both your `database` and `server` installed

### Manage your web application - Supervisor

- Type in your terminal with the address `root@localhost` the line `apt install supervisor` to install supervisor
- Supervisor starts and stops the go binary
- Now, you have `database`, `server` and `supervisor` installed

### Add a new user to account

- Add a user by running `adduser [username]` and follow the instructions
- Give privileges to user to issue sudo commands; type `usermod -aG sudo [username]`
- Test if user is authorized; open and new terminal and type `ssh [username]@IPAddress[copied]` and enter the password for new user
- Confirm if `sudo` privilege is allowed for new user; type `sudo ls` and type password

### Uploading your go app to your server

- Go and [download](https://golang.org/dl/) Go
- Copy the address link of your OS, ie linux
- Run `wget [download of go]`
- Go to [Installation Instruction](https://golang.org/doc/install)
- Select your OS (linux) and copy line tar -C /usr/local -xzf go[major].[minor].[patch].linux-amd64.tar.gz my version 1.17.2 => major.minor.patch respectively
- Export your go path `export PATH=$PATH:/usr/local/go/bin`
- Check your versiion of go with `go version`
- Run `vi .profile` to add your path for the user by pasting `export PATH=$PATH:/usr/local/go/bin`
- Type `esc` and `:wq` to save `.profile`
- Logout and reconnect to server with your credentials
- Ensure you have git installed on your server -- mostly yes, but check with `which git`
- Run `git clone [app repo]`
