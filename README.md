# term7-journal-mod

This is a modded version of the original [Journal theme](https://github.com/tryghost/journal) for [Ghost](https://github.com/tryghost/ghost/), and only used for theme installation!<br>
If you're looking to contribute to Ghost, head over to the main repository [here](https://github.com/TryGhost/Themes).

[Ghost](https://github.com/tryghost/ghost/) advertises the ability to grow the audience of a website into a business. It makes use of Stripe as a payment system and Mailgun as a newsletter provider. With Ghost it is easy to implement a professional newsletter and subscription based content to earn recurring revenue. We don't want to use these features, because we run a site focused on privacy and implementing them would require us to trust third parties with visitor data.

#### IMPORTANT CHANGES:

- We removed the portal with all its signup and subscription options: no newsletter, no subscription based content
- We removed the search functionality because it leverages a third party service: [UNPKG](https://unpkg.com)
- We removed content related to twitter and facebook
- We changed the copyright from © to /| copyme /|
- We added a hook for Matrix Live
- We added jQuery (necessary for Matrix Live)


# Matrix Live

If you want to use Matrix Live for live blogging, you first need to setup a Matrix homeserver (i.e. [Matrix Synapse](https://github.com/matrix-org/synapse)).<br>
Then head over to [Matrix Live](https://live.hello-matrix.net/) and read the instuctions.

You can make your room either open for anyone to join, or configure it to be invite only.<br>
It is up to you.

Please note: because we want to avoid loading external sources, we already added all required scripts and stylesheets to this repository. This means you don't need to add the code as instucted in the Matrix Live tutorial. You only need to inject the address of your homeserver, together with the technical ID of the room you have set up for live blogging, so that it shows up in our modded Ghost theme.

To embedd a live preview of your Matrix room, use the side-wide code injection tool in settings (Ghost Admin). Insert your homeserver's address, your technical room ID and your Matrix username into the script below, then copy it into the Site Footer:

```
<script type="text/javascript">
    $('#matrix').append('<matrix-live homeserver="https://matrix-homeserver.org" room="!your-room-ID-wdRDQ:matrix-homeserver.org" initial-load="60"></matrix-live>');
    $('#matrix-handle').append('@matrix-username:matrix-homeserver.org');
    $('.matrix-access').append('/ -> invite only ...');
</script>
```

# Github & Mastodon

To include links to your Github page and to your Mastodon account, add the following two lines to the javascript in the Site Footer:

```
    $('.gh-author-github').attr('href', 'https://github.com/your-username?tab=repositories');
    $('.gh-author-mastodon').attr('href', 'https://your-mastodon-server/@your-username');
```

In <i>.matrix-access</i> you can define wether or not your Matrix Live Blog is open to the public.

# AN IMPORTANT NOTICE ABOUT PRIVACY:

This is aquote from Ghost's [privacy declaration](https://github.com/TryGhost/Ghost/blob/main/PRIVACY.md):

<em>To easily load member functionality for membership features, Ghost leverages [UNPKG](https://unpkg.com) to provide a CDN for drop-in script known as Portal. If member signups are disabled, no CDN will be injected.</em>

While this is true for the Portal, in order to serch posts, tags and authors, Ghost loads [UNPKG](https://unpkg.com) nevertheless to provide search functionality on the page. We removed all elements and buttons that are related to search functionality because we hoped then the CDN also won't be injected. However, the CDN still is injected into our website. If you look at the page source, it looks like this (the data key always is an individual key for each website):


```
<script defer src="https://unpkg.com/@tryghost/sodo-search@~1.0.0/umd/sodo-search.min.js" data-sodo-search="https://journal.ghost.io/" data-version="1.0.0" data-key="77fa60d37b3ada6d747320b139" crossorigin="anonymous"></script>
```

While search functionality is a nice feature, we don't like having to trust third parties. We want to serve all scripts, fonts, etc. locally.

In oder to stop loading [UNPKG](https://unpkg.com) you will have to host Ghost on your own VPS and change a configuration file.
Change into the directory of your installation and edit '/versions/5.x.x/core/shared/config/defaults.json', i.e.:

`cd /var/www/ghost`<br>
`nano versions/5.4.0/core/shared/config/defaults.json`

Delete these line:

```
"sodoSearch": {
    "url": "https://unpkg.com/@tryghost/sodo-search@~1.0.0/umd/sodo-search.min.js",
    "version": "1.0.0"
},
```

Then restart Ghost:

`ghost restart`

Thats it.<br>
We encourage you to copy, adapt, share and re-distribute!

# Copyright & License

Copyright (c) 2013-2022 Ghost Foundation [& term7] - Released under the [MIT license](LICENSE).
