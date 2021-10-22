### How to host on linode

- Create or sign in to account
- Create linode (the name of their server)
- Choose your package by selecting OS and CPU (default shared minimum 5USD)
- Create your root password
- Click on `create linode`
- Get your provision
- Copy your IP address
- Go to terminal and type `ssh root@IP[copied]` and confirm
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
