# Introduction #

rpi-jabber is a Jabber Bot that allows you to talk to your Raspberry Pi from your jabber/IM client.

You can create your own 'commands' that the jabber daemon will respond to and return you the results.

For example, you could jabber your Pi to turn the lights on and off, or jabber your Pi to tell you what the weather is like at home.


# What you will need #

You will need a jabber account for your Raspberry Pi. If you have access to a jabber server then create your own, otherwise there are a number of free to use jabber servers listed at www.jabber.org

If you prefer you can also use a Google gmail account.


# Installation #

Download the latest jabberd package from here. These are built on a Raspberry Pi using the latest Raspbian image. Put the .deb file somewhere on your Pi and type ` dpkg -i jabberd-1.0_armhf.deb ` as root. This will install the package.

```
cd /tmp
wget http://rpi-jabber.googlecode.com/files/jabberd-1.0_armhf.deb
dpkg -i ./jabberd-1.0_armhf.deb
```

You will end up with an `/etc/jabberd ` directory. The file `jabberd.conf` is the configuration file for the daemon. Edit this file as appropriate with the Jabber or Gmail account details you want your Pi to use.

The `cmds` directory inside `/etc/jabberd` is where the commands live. Two samples are included, `ping` and `address`. If you send ping to your Pi it will respond with Pong. If you send address to your Pi it will return you the Pi's IP address.

**note:** If you want to use a Google account for your Pi you will need to update the `libiksemel3` package on your Raspberry Pi. Google requires the use of TLS and the standard libiksemel package does not have TLS enabled. Download the one from this site and install it as per the jabberd package. This one has TLS enabled correctly.

Once you have installed the package(s) and edited the `/etc/jabberd/jabberd.conf` file you can start the daemon by typing `/etc/init.d/jabberd start` as root from the command line. The jabber daemon will start up and login.

You will probably need to add your Raspberry Pi as a Buddy in your own Jabber account. The jabber daemon will automatically respond to and accept authorisation requests so long as there is an `allow=` line for the requesting jabber id in `/etc/jabberd/jabberd.conf`

Once you have done this, your Raspberry Pi should appear as a buddy in your Jabber client and you should be able to talk to it. Try `ping` or `address`.

# Adding your own commands #

Commands live in the directory `/etc/jabberd/cmds`. These are executable files and can by anything you like, shell scripts, python scripts, php scripts or c code.

When you send your Pi a message it takes the first word of the message and looks for a matching filename in `/etc/jabberd/cmds`. If it finds one, and it is executable (ie. it must be chmod 755 or similar), it will run it and pass the rest of the jabber message as command line arguments. Anything the command outputs will be sent back to the jabber client as a response.

For example, if you sent `hello one two three` to your Pi, it would look for an executable file called `/etc/jabberd/cmds/hello` and run it with `one two three` as command line arguments.


# Getting your Raspberry Pi to send you messages #

Included in the package is a utility program called `jabber`. You can use this to get your Pi to send you a jabber message. For example, you might have a cron job (timed job) that gets the status of a GPIO. You can use the `jabber` utility to send that to you.

`jabber user@somedomain This is a message`