# Being Graceful

This page lists several ways of doing things _gracefully_ - that is, doing things in such a way that disruption to listeners, presenters and officers is as minimal as possible (Not, doing things like Michael Grace).

## Timing your breakages

When about to do something that might break things (for example: website builds, website updates, configuration changes), try to time them so that they occur during a URY Jukebox slot. This way, at worst you won't have a presenter on your case when things keel over, and at best nobody will be listening when things go belly-up.

An exception to this rule is maintenance to the Jukebox itself, or resolving an urgent security issue.

## SHOOP

SHOOP, or _System Hug/Official Outage Period_, is what we call specially arranged time where we can break stuff/fix the servers (referred to lovingly as "hugs").

It's always best to request SHOOP from the Programme Controller (see below) if you need it. We're trying to get some form of official SHOOP system going, but this is bound to be contentious.

## Letting the Programme Controller know

If you need to take a show or set of shows off-air, then try to let the Programme Controller know **72 hours** in advance so they can OK it and give the presenters 48 hours notice.

## Reloading the web server

_Always_ try ''apache2ctl graceful'' before resorting to ''apache2ctl restart'', for things such as reloading config files.

## Disabling the website

Sometimes there comes a time when you'll have to close the website off completely. In this case, unless you really need to pull the web server down, the graceful approach is to switch the website over to disabled mode.

If you're doing anything that'll take the databases down (for example, upgrades on [URYFS1](computing/comp-machines/uryfs1)), you'll need to disable the website. This is because the website uses the databases a lot, and will keel over without them.

Website disables are done by setting the **disabled-reason** [website property](computing/website/properties). **IMPORTANT: If you're taking the database down, you **//MUST//** set disabled-reason via a property override (details on how to do this are in the properties wiki page)!**
