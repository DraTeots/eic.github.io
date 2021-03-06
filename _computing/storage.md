---
title: NextCloud
description: NextCloud
name: storage
layout: default
level: 0
---

{% include layouts/title.md %}

#### Pre-generated Monte Carlo data at BNL

The EIC group at BNL has a large amount (13 TB and growing) of
pre-generated events from Pythia, Milou, Djangoh, and others available
for download via NextCloud/BNLbox.
To access this data, no lab association is necessary. Simply contact Kolja Kauder, kkauder@bnl.gov, and request a guest account.

You will receive an invitation where you can choose your password. From then on, you can log into
https://bnlbox.sdcc.bnl.gov
and find everything in the ```EIC_Data``` directory via the web interface.


##### Download from the command line ####

Instructions for using curl, cadaver, and a script that can download entire directories can be found
[here](https://racfjira.atlassian.net/wiki/spaces/BBD/pages/604307461/How+To+Access+BNL+Box+From+The+Command+Line).

A few additional points that may simplify life:
* A copy of the ```copy_bnl_box.rb``` script is found inside ```EIC_Data```.
* Note to RCF users, you can access this data directly at
```
/gpfs02/eic/DATA
```
* IMPORTANT: As the documentation notes, guest accounts work simply with the email + password combination. However, do choose a reasonably strong password, but do NOT reuse a password you use for any other service! If you use the command line interface, this password may well be sent unencrypted and/or be in a clear-text file on your computer.
* Assuming your guest account was created for myemail@myprovider.com, the path to the directory will always have a specific form and you can use an environment variable as a shortcut:
```
export eicdataurl='https://bnlbox.sdcc.bnl.gov/remote.php/dav/files/myemail@myprovider.com/EIC_Data'
```
or
```
setenv eicdataurl 'https://bnlbox.sdcc.bnl.gov/remote.php/dav/files/myemail@myprovider.com/EIC_Data'
```
This way, you can (with a suitably configured .netrc) use something like:
```
curl -O -n ${eicdataurl}/copy_bnl_box.rb
```


#### Adding to the collection ####
There is more than enough space available to fulfill all storage needs
of the Yellow Report effort as reported in the recent poll to working
groups. To make use of it, please contact the software working group
so we can determine together the most efficient way to upload your
data and make it accessible to the whole community!
