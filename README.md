## dgamelaunch

dgamelaunch is a network-based game shell best known for being used on [nethack.alt.org](nethack.alt.org). This repository provides a set of configuration files suitable for the roguelike game ToME 2. The official dgamelaunch repository is located at [https://github.com/paxed/dgamelaunch](https://github.com/paxed/dgamelaunch)

### Setting up a ToME 2 server

The following commands will get a ToME 2 server up and running as quickly as possible on Debian Jessie.

Compile and install dgamelaunch:
```bash
sudo apt-get install git autoconf sqlite3 libsqlite3-dev bison flex libncursesw5 libncursesw5-dev libncurses5-dev
git clone https://github.com/adamreiser/dgamelaunch.git
cd dgamelaunch && checkout tome
./autogen.sh --enable-sqlite --enable-shmem --with-config-file=/opt/dgamelaunch.conf 
sudo cp examples/dgamelaunch.conf /opt/dgamelaunch.conf
make && sudo make install
```

Compile and install ToME 2. This version has been configured to work with dgamelaunch and contains security fixes for a multiuser environment:

```bash
sudo apt-get install cmake build-essential libboost-all-dev libjansson-dev pkg-config
git clone https://github.com/adamreiser/tome2.git
cd tome2 && checkout dgamelaunch 
cmake -DSYSTEM_INSTALL:BOOL=true . 
make && sudo make install 
```

Finally, create the server environment and configure anonymous access. ToME must be installed on the system for dgl-create-chroot to work.

```bash
sudo ./dgamelaunch/dgl-create-chroot
sudo chmod 4755 /usr/local/sbin/dgamelaunch # needed for chroot
adduser --no-create-home --shell /usr/local/sbin/dgamelaunch tome
```
Set the password to something simple (it's going to be public) and check that ssh is enabled. Users will now be able to log in with `ssh tome@host`.


