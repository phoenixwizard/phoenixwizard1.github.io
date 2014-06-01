---
layout: post
title: The spinning Arrow in WP Contact Form 7
comments: true
permalink: spinning-arrow-wp-contact-form-7
---

So I was using the [Hickory](http://themeforest.net/item/hickory-a-wordpress-magazine-theme/5437524) for a project along with [Contact Form 7 Plugin](http://wordpress.org/plugins/contact-form-7/) and [Easy WP SMTP](http://wordpress.org/support/view/plugin-reviews/easy-wp-smtp). I used this setup for contact form and a couple of forms in the blog.

Now comes the interesting part !! When I submitted the form, the arrows in the bottom would keep on spinning !! The mail was sent in backend but the user saw the infinitely spinning arrows.
![The spinning Image](https://dl.dropboxusercontent.com/u/56592400/Screen%20Shot%202014-06-02%20at%2012.47.08%20am.png)
If you go to the support forum, you will see maaaannnnyyyy posts about the same. Everyone has their hacky solutions !! Finally a gentleman pointed out where to look the error !! I looked into the ajax call the form was making and realised that the response was NOT a JSON. But an amalgation of errors and a JSON.

I was getting :

    SMTP -> FROM SERVER:220 mx.google.com ESMTP ko10sm16651579pbd.52 - gsmtp

    <br />SMTP -> FROM SERVER: 250-mx.google.com at your service, [128.199.209.28]
    250-SIZE 35882577
    250-8BITMIME
    250-AUTH LOGIN PLAIN XOAUTH XOAUTH2 PLAIN-CLIENTTOKEN
    250-ENHANCEDSTATUSCODES
    250 CHUNKING

    <br />SMTP -> FROM SERVER:250 2.1.0 OK ko10sm16651579pbd.52 - gsmtp

    <br />SMTP -> FROM SERVER:250 2.1.5 OK ko10sm16651579pbd.52 - gsmtp

    <br />SMTP -> FROM SERVER:354  Go ahead ko10sm16651579pbd.52 - gsmtp

    <br />SMTP -> FROM SERVER:250 2.0.0 OK 1401650231 ko10sm16651579pbd.52 - gsmtp

    <br />SMTP -> FROM SERVER:221 2.0.0 closing connection ko10sm16651579pbd.52 - gsmtp

    <br />{"mailSent":true,"into":"#wpcf7-blah-blah","captcha":{"captcha-blah blah":"http:\/\/blahblah.com\/wp-content\/uploads\/wpcf7_captcha\/blah blah.png"},"message":"Your message was sent successfully. Thanks."}

And What I needed was just

    {"mailSent":true,"into":"#wpcf7-blah-blah","captcha":{"captcha-blah blah":"http:\/\/blahblah.com\/wp-content\/uploads\/wpcf7_captcha\/blah blah.png"},"message":"Your message was sent successfully. Thanks."}

Then somebody pointed out that Easy WP SMTP has an *Enable SMTP Debug* option !! Voila !! So this was causing the errors getting printed in response to the ajax call and rendering the response unreadable to Contact Form !!

![The perfect Image](https://dl.dropboxusercontent.com/u/56592400/Screenshot%202014-06-02%2000.49.16.png)

I spent quiet some time debuging this !! Hope this saves someone some time in the future !!

Peace !!