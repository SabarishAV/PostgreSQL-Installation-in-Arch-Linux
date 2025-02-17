# PostgreSQL Installation in Arch Linux

<p>
This guide provides step-by-step instructions for installing and setting up PostgreSQL on Arch Linux. Follow these instructions to easily install PostgreSQL using <code>pacman</code>, configure the service, and start using your database. Perfect for developers or anyone looking to run PostgreSQL on Arch-based systems.
</p>

<p>Follow the commands below to install PostgreSQL on your system:</p>

<ul>
<li>Update your system (to ensure all packages are up-to-date):</li>

```bash
sudo pacman -Syu
```
<li>Install PostgreSQL:</li>

```bash
sudo pacman -S postgresql
```
<li>Initialize the PostgreSQL database:</li>

```bash
sudo -u postgres initdb --locale $LANG -D /var/lib/postgres/data
```
<li>Start and enable the PostgreSQL service to run at boot:</li>

```bash
sudo systemctl enable --now postgresql
```
<li>To check the status of the PostgreSQL service, you can use the following command (<strong>optional</strong>):</li>

```bash
sudo systemctl status postgresql
```
<li>To set a password for the <code>postgres</code> user, follow these steps (<strong>optional</strong>):</li>
<ol>
<li>Switch to the postgres user:</li>

```bash
sudo -i -u postgres
```
<li>Set the password using the <code>psql</code> command: Run the following command to enter the PostgreSQL command line interface:</li>

```bash
psql
```
<li>Set the password: Inside the <code>psql</code> shell, run the following SQL command to set a password for the <coed>postgres</code> role:</li>

```bash
ALTER USER postgres PASSWORD 'your_password_here';
```
<p>Replace <code>'your_password_here'</code> with the password you'd like to set for the <code>postgres</code> user.</p>
<li>Exit the <code>psql</code> shell:</li>

```bash
\q
```
<li>Exit the <code>postgres</code> user shell:</li>

```bash
exit
```
</ol>
<li>
To restrict PostgreSQL so that it only accepts a specific password for the <code>postgres</code> user (or any other user), you'll need to modify the PostgreSQL authentication settings. Here's how you can configure it properly(<strong>optional</strong>):
</li>
<ol>
<li>Edit the PostgreSQL authentication configuration file (<code>pg_hba.conf</code>):</li>
<p>
The authentication method for PostgreSQL is controlled by the <code>pg_hba.conf</code> file, which is located in the PostgreSQL data directory. The exact path depends on your system, but it's typically found at <code>/var/lib/postgres/data/pg_hba.conf</code>
</p>

```bash
sudo nano /var/lib/postgres/data/pg_hba.conf
```
<li>Find the relevant authentication method:</li>
<p>Make these changes as shown below.</p>

```bash
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     trust
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust
# IPv6 local connections:
host    all             all             ::1/128                 trust
```

```bash
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     md5
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
# IPv6 local connections:
host    all             all             ::1/128                 md5
```
<p>
The <code>trust</code> method means PostgreSQL will authenticate any user without requiring a password. To ensure that PostgreSQL uses password authentication, change <code>trust</code>  to <code>md5</code>  (which means MD5 password authentication).
</p>
<li>Reload PostgreSQL to apply the changes:</li>

```bash
sudo systemctl reload postgresql
```
</ol>
<li>Switch to the postgres user to create a PostgreSQL role (optional, if needed):</li>

```bash
sudo -i -u postgres
```
<li>Access the PostgreSQL prompt:</li>

```bash
psql -U postgres -W
```
<li>You can enter queries here:</li>
<li>Exit the <code>psql</code> shell:</li>

```bash
\q
```
<li>Exit the <code>postgres</code> user shell:</li>

```bash
exit
```
</ul>