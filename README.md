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
- We created a custom dynamic tags.hbs and edited routes.yaml to create an overview page for all tags used in posts


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

# Custom Tags Page

To include a custom dynamic tags page, we created [tags.hbs](tags.hbs) as a modified version of [default.hbs](default.hbs) and edited our `routes.yaml` to look like this:

```
routes:
  /tags/: tags

collections:
  /:
    permalink: /{slug}/
    template: index

taxonomies:
  tag: /tag/{slug}/
  author: /author/{slug}/
```

If you want to show an overview of all tags used on your page on an overview page, you have to first download `routes.yaml` from your Ghost Publishing Settings - Labs Page (at the bottom of `https://your-domain-name/ghost/#/settings/labs`). Edit the file on your computer and upload your modified version of `routes.yaml` via your Ghost Publishing Settings Page.

To include tags into your page navigation, you then just need to add it in your Ghost Publishing Settings - Navigation Page page as `https://your-domain-name/tags/`

# AN IMPORTANT NOTICE ABOUT PRIVACY

This is a quote from Ghost's [privacy declaration](https://github.com/TryGhost/Ghost/blob/main/PRIVACY.md):

<em>To easily load functionality for membership features & search, Ghost leverages [JSDELIVER](https://unpkg.com) to provide a CDN for drop-in scripts.</em>

Even if you don't use functionality like the Signup Portal, in order to search posts, tags and authors, Ghost loads JSDELIVER nevertheless. We removed all elements and buttons in our theme design that are related to search functionality because we hoped in consequence the CDN also won't be injected anymore. However, the CDN still is injected into our website. If you look at the page source, it looks like this (the data key always is an individual key for each website):


```
<script defer src="https://cdn.jsdelivr.net/npm/@tryghost/sodo-search@~1.1/umd/sodo-search.min.js" data-key="77fa60d37b3ada6d747320b139" data-styles="https://cdn.jsdelivr.net/npm/@tryghost/sodo-search@~1.1/umd/main.css" data-sodo-search="https://term7.info/" crossorigin="anonymous"></script>
```

While search functionality is a nice feature (at the very latest when the content on your site grows), and even though JSDELIVER is an open source project that is for example used by Modzilla, we still don't like having to trust third parties. Our site does not need a CDN. To see how JSDELIVER works, check out their own [Infographic](https://www.jsdelivr.com/network/infographic). There you also see in which way 3rd parties are integrated... The injected CDN further loads additional resources (Javascript and CSS stylesheets), over which we have no direct control. We do believe JSDELIVR is a good project, but eventually there always is trust involved if you integrate 3rd party scripts that will be updated by this 3rd party and thus automatically integrated into your installation (to improve the software, make it more secure, eliminate bugs, etc.). For privacy reasons we want to serve all scripts, fonts, etc. locally.
However a CDN can make a lot of sense when you run a website that has a lot of traffic, if you want to have subscribers to your content, implement a newsletter, etc. In this case we recommend you use the standard installation of ghost and don't temper with its core configuration.

If you do want to stop loading [JSDELIVER](https://www.jsdelivr.com/) you will have to host Ghost on your own VPS and change a configuration file. You can find instructions on how to install Ghost [here](https://ghost.org/docs/install/).

Then, log into your VPS, enter the directory of your ghost installation and edit '/versions/5.x.x/core/shared/config/defaults.json', i.e.:

`cd /var/www/ghost`<br>
`nano config.production.json`

To disable all features that rely on JSDELIVR these lines:

```
  "portal": {
    "url": ""
  },
  "sodoSearch": {
    "url": ""
  },
  "comments": {
    "url": ""
  },
  "editor": {
    "url": ""
  }
```

Here you can also disable other functions that are enabled by default and outlined in Ghost's [privacy declaration](https://github.com/TryGhost/Ghost/blob/main/PRIVACY.md).<br>
It is up to you!

```
  "privacy": {
    "useUpdateCheck": false,
    "useGravatar": false,
    "useRpcPing": false,
    "useStructuredData": false
  },
```

Then restart Ghost:

`ghost restart`

We encourage you to copy, adapt, share and re-distribute!

# Copyright & License

Copyright (c) 2013-2022 Ghost Foundation [& term7] - Released under the [MIT license](LICENSE).
