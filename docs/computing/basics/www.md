# WWW Access

Most of our Internet resources are available on [http://ury.york.ac.uk](http://ury.york.ac.uk), which is a publicly available server that you should have no problems getting to. In order to access Web resources on any other URY server, however, you might need to do something extra.

## Campus-Restricted Servers

See the [server list](computing/comp-hardware/servers) for the current list of these.

If you're connecting from the university campus network, you should have no problems getting to these. However, if you're off the University campus, you'll need to use one of these:

### Use the university proxy server

This is probably the easiest way to get access to campus-only Web servers. (It also lets you into things you'd normally need to be on campus for too, like getting free copies of academic papers!)

Just set your **proxy server** in your Web browser configuration to ''http://wwwcache.york.ac.uk:8080'', and you should be able to get through. (This will work for both HTTP and HTTPS -- you'll need to set both of these proxies for full access to our stuff)

wwwcache will ask you for a username and password: these are your Computing Services/main university details (**NOT** your Computer Science or URY details!)

### Run a Web browser on a campus machine

For quick trips, you could always remotely run a Web browser. For example, it's possible to SSH into milan and run the lynx text browser (graphical browsers might be possible with X forwarding, but in the author's experience they're way too slow to be usable).

### Try to access the resource from a public machine

Some of our campus-only Web resources are actually available through the public Web servers, through the miracles of Web server proxying. For example, the development website (''urybsod, port 8000''') is available through [http://ury.org.uk/dev](http://ury.org.uk/dev).

## URY Internal Servers

At the time of writing, there's nothing you should need to access through HTTP on our internal servers, but if you did need to, you'd have to use [SSH](SSH Access) or something to tunnel into them.
