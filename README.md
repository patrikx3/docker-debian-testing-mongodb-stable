[//]: #@corifeus-header

 

[![Donate for Corifeus / P3X](https://img.shields.io/badge/Donate-Corifeus-003087.svg)](https://paypal.me/patrikx3) [![Contact Corifeus / P3X](https://img.shields.io/badge/Contact-P3X-ff9900.svg)](https://www.patrikx3.com/en/front/contact) [![Corifeus @ Facebook](https://img.shields.io/badge/Facebook-Corifeus-3b5998.svg)](https://www.facebook.com/corifeus.software)   [![Build Status](https://travis-ci.com/patrikx3/docker-debian-testing-mongodb-stable.svg?branch=master)](https://travis-ci.com/patrikx3/docker-debian-testing-mongodb-stable) [![Uptime Robot ratio (30 days)](https://img.shields.io/uptimerobot/ratio/m780749701-41bcade28c1ea8154eda7cca.svg)](https://uptimerobot.patrikx3.com/)

# ✨ Debian Bullseye / Bookworm / Testing / SID MongoDB and MongoDB Tools build stable 


                        
[//]: #@corifeus-header:end

It is basically a built for the latest MongoDB for Debian.
  
**Unfortunately, I never tested it on any other architecture, but `x64`, so, it is possible it will not build other CPU types. [See this link.](https://docs.mongodb.com/manual/installation/#mongodb-supported-platforms)**
  
The current version is the r4.2.4 build (https://docs.mongodb.com/manual/release-notes/).

There is a newer version `4.3.0`, but given, we use `NoSQLBooster`, it only works with `4.2.0` and the `4.2.x` is the stable, the next stable will be `4.4.0`, `4.6.0` and so on...

### Warning

It will remove all ```mongodb*``` apt packages in ```./scripts/build-server.sh``` and ```/etc/systemd/system/mongodb-server.service``` is replaced.  

It installs the required `apt` dependencies and generates the ```SystemD``` service and makes it enabled.  
  
Check, if the build works (building is below). If there is an error, of course, you will not deploy on your server. But, if it builds, then it puts the binaries into `/usr/bin` and you are done. You might want to create a backup first, but I never had an error to make the database into an unstable state. 
  
There was a Docker file to build in a container, but it was slower and I deprecated.
  
Before, the script was building everything including unit tests, but it took so long, so I left it out, now, it can build on a 4/8 cores/threads Intel 7700k in 1-2 hours.

## Scripts for building

It can work with `sudo`, but the best if you are ```root```. Of course, you can check the ```code```, there is no ```harm``` for sure!

```bash
git clone https://github.com/patrikx3/docker-debian-testing-mongodb-stable
cd docker-debian-testing-mongodb-stable
```

If below you get an error, please create an ```issue```, because it is possible, I have not added a package, because my server was already there, but I will add in it for you for sure with ```apt```.  

The default jobs for building is by the number of threads in the cpu, but you can override as a `CORES` variable. 

### 1. Build MongoDB Server

The command:
```bash
sudo ./scripts/build-server.sh
```

From:  
https://github.com/mongodb/mongo/wiki/Build-Mongodb-From-Source

All defaults are in the config, that MongoDB uses:  
* /var/log/mongodb - log
* /var/lib/mongodb - data

It generates everything, all you have to do:

```bash
sudo ./scripts/build-server.sh r4.2.4
# if you want to specify how many cores you wanna use do like
sudo CORES=4 ./scripts/build-server.sh r4.2.4
```

### 2. Build MongoDB Tools

The command:
```bash
./scripts/build-tools.sh
```

It generates and install GoLang and builds the tools that you find them in:    
https://github.com/mongodb/mongo-tools

Then, it puts all tools into the default Debian ```/usr/bin``` directories.

The exact command is like:
```bash
sudo ./scripts/build-tools.sh r4.2.4
```

### 3. Start the services

Before you start the database, **but after the build** , you are required to create a config (unless, you already have it), a skeleton is here:  
```text
artifacts/root-filesystem/etc/mongodb.conf
```


#### Add safety to the mongodb config file

```bash
sudo cp ./artifacts/root-filesystem/etc/mongodb.conf /etc/mongodb.conf
sudo chmod o-rwx /etc/mongodb.conf
sudo chown mongodb:mongodb /etc/mongodb.conf
```

After you created the config, you start the database like:  
```service mongodb-server start``` or ```service mongodb-server restart```



<!---

### 3. Sometimes check the kernel


The command:
```bash
./scripts/check-kernel.sh
```

It the kernel have changed, it better to re-build the server and the tools.

Right now the stable MongoDB 4.0.0 doesn't show the kernel version anymore

# Add user

```bash
cp ./artifacts/root-filesystem/etc/systemd/system/mongodb-server.service /etc/systemd/system/mongodb.service
cp ./artifacts/root-filesystem/etc/mongodb.conf /etc/mongodb.conf
sudo useradd mongodb -d /var/lib/mongodb -s /bin/false || true
sudo -u mongodb mkdir -p /var/lib/mongodb
sudo chmod o-rwx -R /var/lib/mongodb
systemctl daemon-reload
systemctl enable mongodb-server
service mongodb-server start
```

--->

[//]: #@corifeus-footer

---

🙏 This is an open-source project. Star this repository, if you like it, or even donate to maintain the servers and the development. Thank you so much!

Possible, this server, rarely, is down, please, hang on for 15-30 minutes and the server will be back up.

All my domains ([patrikx3.com](https://patrikx3.com) and [corifeus.com](https://corifeus.com)) could have minor errors, since I am developing in my free time. However, it is usually stable.

**Note about versioning:** Versions are cut in Major.Minor.Patch schema. Major is always the current year. Minor is either 4 (January - June) or 10 (July - December). Patch is incremental by every build. If there is a breaking change, it should be noted in the readme.


---

[**P3X-DOCKER-DEBIAN-TESTING-MONGODB-STABLE**](https://pages.corifeus.com/docker-debian-testing-mongodb-stable) Build v2020.4.112

[![Donate for Corifeus / P3X](https://img.shields.io/badge/Donate-Corifeus-003087.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=QZVM4V6HVZJW6)  [![Contact Corifeus / P3X](https://img.shields.io/badge/Contact-P3X-ff9900.svg)](https://www.patrikx3.com/en/front/contact) [![Like Corifeus @ Facebook](https://img.shields.io/badge/LIKE-Corifeus-3b5998.svg)](https://www.facebook.com/corifeus.software)


## P3X Sponsor

[IntelliJ - The most intelligent Java IDE](https://www.jetbrains.com/?from=patrikx3)

[![JetBrains](https://cdn.corifeus.com/assets/svg/jetbrains-logo.svg)](https://www.jetbrains.com/?from=patrikx3)




[//]: #@corifeus-footer:end
