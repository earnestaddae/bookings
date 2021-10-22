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
- Run `git clone [app repo]` NB: ensure you are cloning with **HTTPS** and not SSH

### Now you have app cloned, connect to your database

- cd in `/etc/postgresql/[version]/main`
- Open and make changes to `pg_hba.conf` with `sudo vi pg_hba.conf`
- Go to `host all all 127.0.0.1/32 md5` under the comment **# IPv4 local connections:** and change md5 to `trust`
- Go to `host all all ::1/128 trust` under the comment **# IPv6 local connections:** and change md5 to `trust`
- Type `:wq` to save the file `pg_hba.conf`
- Stop postgres with `sudo service postgresql stop` and restart with `sudo service postgresql start`
- Make sure it is running with `ps ax | grep postgr`

---

#### Connection remote db to local

- Go the database GUI you are using, mine DBeaver
- Click on `New Database Connection` and select `PostgreSQL`
- Click on `SSH` and check `Use SSH Tunnel`
- Under `settings` type your **linode server IP address** provided in th **Host/IP** field
- Enter the credentials used to create the ssh account earlier for the new user
- Make sure, you are logout of your remote server, navigate to `Main` and click on `Test Connection`
- Once the database connection is successful click finish
- You should see your new postgres with your linode server IP address
- Create a new database name under your new postgres - linode IP Address
- Log back in to your remote server in your terminal with `ssh username@linodeprovidedIPAddress`
- Type `cp database.yml.example database.yml`, and make changes with your db credentials with username being `postgres`

---

#### Generate migration by installing dependencies

- Install pop for db migration as used in project with `go get github.com/gobuffalo/pop/...` to ensure soda is available
- `ls ~/go/bin` should yield `soda`
- In your home directory run `vi .profile`, add `export PATH=$PATH:~/go/bin`, save and export it in your terminal to add `soda` to path
- cd `app cloned earlier` directory
- Run `soda migrate` to migrate your database
- You should have all your tables migrated to linode server db
- Still in your `cloned app` build the app with `go build -o [appName] cmd/web/*.go` to create executable file
- ALL SET!

### Connect application to web server

- Visit http://[linodeprovidedIPAddress] to see app
- Go your terminal and login to your remote server with `ssh username@linodeprovidedIPAddress`
- `cd /etc/caddy/` to find `Caddyfile`
- `sudo mv Caddyfile Caddyfile.dist`
- Create your own caddyfile with `sudo vi Caddyfile`, stick to name **Caddyfile**
- In your Caddyfile, add the configurations from _caddfile.md_
- Create `conf.d` directory, cd into it and `book.conf` file and add import `static` and `security` from _caddfile.md_
- Create in `/var/www/book/logs/caddy-access.log`
- `mv ~/bookings book`
- `sudo chmod 777 logs`
- Restart caddy `sudo service caddy restart`
- Caddy status `sudo service caddy status`
- from `/var/www/book` run `./bookings -dbname=database -dbpass=password -dbuser=dbuser`

### Supervior takes over

- cd etc/supervisor
- cd conf.d
- create book.conf file
- add the configuration in after --- in `book.conf` and save
- run `sudo supervisorctl`
- run `status`
- run `update`
