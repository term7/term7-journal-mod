# term7-journal-mod

This is a modded version of the original [Journal theme](https://github.com/tryghost/journal) for [Ghost](https://github.com/tryghost/ghost/), and only used for theme installation!<br>
If you're looking to contribute to Ghost, head over to the main repository [here](https://github.com/TryGhost/Themes).

[Ghost](https://github.com/tryghost/ghost/) advertises the ability to grow the audience of a website into a business. It makes use of Stripe as a payment system and Mailgun as a newsletter provider. With Ghost it is easy to implement a professional newsletter and subscription based content to earn recurring revenue. We don't want to use these features, because we run a site focused on privacy and implementing them would require us to trust third parties with visitor data.

#### IMPORTANT CHANGES:

- We removed the portal with all its signup and subscription options: no newsletter, no subscription based content
- We removed content related to twitter and facebook
- We changed the copyright from © to /| copyme /|
- We added a hook for Matrix Live
- We added jQuery (necessary for Matrix Live)

# Matrix Live

If you want to use Matrix Live, you first need to setup a Matrix homeserver (i.e. [Matrix Synapse](https://github.com/matrix-org/synapse)).<br>
Then head over to [Matrix Live](https://live.hello-matrix.net/) and read the instuctions.

You can make your room either open for anyone to join, or configure it to be on invite only. It is up to you.

Please note: we already added all required scripts and stylesheets to this repository. You only need to inject the address of your homeserver and the technical ID of the room you have set up for live blogging so that it shows up in our modded Ghost theme.

To embedd a live preview of your Matrix room, use the side-wide code injection tool in settings (Ghost Admin). Insert your homeserver's address and your technical room ID into this script and copy it to the Site Footer:

```
<script type="text/javascript">
    $('#matrix').append('<matrix-live homeserver="https://your-matrix-homeserver.org" room="!your-room-ID-wdRDQ:your-matrix-homeserver.org" initial-load="60"></matrix-live>');
</script>
```

Thats it.
We encourage you to copy, adapt, share and re-distribute!

# Copyright & License

Copyright (c) 2013-2022 Ghost Foundation [& term7] - Released under the [MIT license](LICENSE).
