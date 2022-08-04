# term7-journal-mod

This is a modded version of the original [Journal theme](https://github.com/tryghost/journal) for [Ghost](https://github.com/tryghost/ghost/), and only used for theme installation!<br>
If you're looking to contribute to Ghost, head over to the main repository [here](https://github.com/TryGhost/Themes).

[Ghost](https://github.com/tryghost/ghost/) advertises the ability to grow the audience of a website into a business. It makes use of [Stripe](https://stripe.com/en-gb-us) as a payment system, [JSDELIVER](https://www.jsdelivr.com/) as a CDN to enable Portal login & registrations, as well as [Mailgun](https://www.mailgun.com/) as a newsletter provider. With Ghost it is easy to implement a professional newsletter and subscription based content to earn recurring revenue. We don't want to use these features, because we run a site focused on privacy and implementing them would require us to trust third parties with visitor data.

#### IMPORTANT CHANGES:

- We removed the portal with all its signup and subscription options: no newsletter, no subscription based content
- We removed the search functionality
- We removed content related to Twitter and Facebook
- We changed the copyright from © to /| copyme /|
- We added a hook for Matrix Live
- We added jQuery (necessary for Matrix Live)


# Matrix Live

If you want to use Matrix Live for live blogging, you first need to setup a Matrix homeserver (i.e. [Matrix Synapse](https://github.com/matrix-org/synapse)).<br>
Then head over to [Matrix Live](https://live.hello-matrix.net/) and read the instuctions.

You can make your room either open for anyone to join, or configure it to be invite only.<br>
It is up to you.

Please note: because we want to avoid loading external sources, we already added all required scripts and stylesheets to this repository. This means you don't need to add the code as instucted in the Matrix Live tutorial. You only need to inject the address of your homeserver, together with the technical ID of the room you have set up for live blogging, so that it shows up in our modded Ghost theme.

To embedd a live preview of your Matrix room, use the site-wide code injection tool in settings (Ghost Admin). Insert your homeserver's address instead of `matrix-homeserver.org`, your technical room ID instead of `!your-room-ID` and your Matrix username instead of `matrix-username` into the script below, then copy it into the Site Footer:

```
<script type="text/javascript">
    $('#matrix').append('<matrix-live homeserver="https://matrix-homeserver.org" room="!your-room-ID:matrix-homeserver.org" initial-load="60"></matrix-live>');
    $('#matrix-handle').append('@matrix-username:matrix-homeserver.org');
    $('.matrix-access').append('/ -> invite only ...');
</script>
```

In <i>.matrix-access</i> you can for example define wether or not your Matrix Live Blog is open to the public.

# Github & Mastodon

To include links to your Github page and to your Mastodon account, also add the following two lines to the javascript in the Site Footer:

```
    $('.gh-author-github').attr('href', 'https://github.com/your-username');
    $('.gh-author-mastodon').attr('href', 'https://your-mastodon-server/@your-username');
```

Again, don't forget to change `your-username` and `your-mastodon-server` to the appropriate values!

# AN IMPORTANT NOTICE ABOUT PRIVACY

This is a quote from Ghost's [privacy declaration](https://github.com/TryGhost/Ghost/blob/main/PRIVACY.md):

<em>To easily load functionality for membership features & search, Ghost leverages [JSDELIVER](https://unpkg.com) to provide a CDN for drop-in scripts.</em>

Even if you don't use functionality like the Signup Portal, in order to search posts, tags and authors, Ghost loads JSDELIVER nevertheless. We removed all elements and buttons in our theme design that are related to search functionality because we hoped in consequence the CDN also won't be injected anymore. However, the CDN still is injected into our website. If you look at the page source, it looks like this (the data key always is an individual key for each website):


```
<script defer src="https://cdn.jsdelivr.net/npm/@tryghost/sodo-search@~1.1/umd/sodo-search.min.js" data-key="77fa60d37b3ada6d747320b139" data-styles="https://cdn.jsdelivr.net/npm/@tryghost/sodo-search@~1.1/umd/main.css" data-sodo-search="https://term7.info/" crossorigin="anonymous"></script>
```

While search functionality is a nice feature, and even though JSDELIVER is an open source project that is for example used by Modilla, we still don't like having to trust third parties. Our site does not need a CDN. To see how JSDELIVER works, check out their own [Infographic](https://www.jsdelivr.com/network/infographic). There you also see in which way 3rd parties are integrated... The injected CDN further loads additional resources (Javascript and CSS stylesheets), over which we have no direct control. For privacy reasons we want to serve all scripts, fonts, etc. locally.

In oder to stop loading [JSDELIVER](https://www.jsdelivr.com/) you will have to host Ghost on your own VPS and change a configuration file. You can find instructions on how to install Ghost [here](https://ghost.org/docs/install/).

Then, log into your VPS, enter the directory of your ghost installation and edit '/versions/5.7.0/core/shared/config/defaults.json', i.e.:

`cd /var/www/ghost`<br>
`nano versions/5.7.0/core/shared/config/defaults.json`

Delete these lines:

```
"sodoSearch": {
    "url": "https://cdn.jsdelivr.net/npm/@tryghost/sodo-search@~{version}/umd/sodo-search.min.js",
    "styles": "https://cdn.jsdelivr.net/npm/@tryghost/sodo-search@~{version}/umd/main.css",
    "version": "1.1"
},
```

You could as well delete all configuration lines that load external URL's. It is up to you!<br>
In our experience it has been good enough to only delete the lines related to <em>sodoSearch</em>...

Then restart Ghost:

`ghost restart`

Unfortunately we relised that starting with Ghost version 5.5.0 (which is when Ghost changed from UNPKG to JSDELIVR), deleting these lines will break Ghost. Code injection in the site header stopped working...
Until this issue is resolved we recommend to revert the update to Ghost Version 5.4.0 with these commands:

`cd /var/www/ghost`<br>
`rm -R versions*`<br>
`ghost update 5.4.0 --force`

Then in order to stop UNPKG CDN from being injected, edit <em>defaults.json</em>:

`nano versions/5.4.0/core/shared/config/defaults.json`

And delete these lines:

```
"sodoSearch": {
    "url": "https://unpkg.com/@tryghost/sodo-search@~1.0.0/umd/sodo-search.min.js",
    "version": "1.0.0"
    },
```

Thats it.<br>
We encourage you to copy, adapt, share and re-distribute!

# Copyright & License

Copyright (c) 2013-2022 Ghost Foundation [& term7] - Released under the [MIT license](LICENSE).
