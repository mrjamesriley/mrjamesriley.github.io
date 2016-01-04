---
layout: post
title: "Capistrano ignoring ssh public key"
date: 2010-11-30 05:42
categories: blog
---
An issue that had me stumbled a while back and one of which I was reminded of
this morning: the initially frustrating issue of capistrano repeatedly asking
for my server ssh password. Now connecting to the deployment server via keyless
ssh works without fail, the deploy config file seems fine and you can connect
to your source code server via keyless ssh &ndash; so what gives?

A key aspect to remember is that the purpose of capistrano is to run commands
on your remote deployment server as if you were running them locally on the
server itself. So when we reach the task of grabbing the code from your chosen
code repository (cloning from git for example), the command is being called
from the deployment server and so it&rsquo;s this connection that requires the ssh
connection set up is order to avoid passwords.

The solution here is to set up the ssh key between your deployment server and
your source code repository server to authorise the connection, and this is
done in the usual way: running ssh-keygen on the deployment server, then
copying the generated .ssh/id_rsa.pub into a new line of your source code
servers .ssh/authorized_keys2 file. The details here may vary depending on your
setup.

Done, no more pestering &ndash; capistrano is supposed to improve productivity after
all right?
