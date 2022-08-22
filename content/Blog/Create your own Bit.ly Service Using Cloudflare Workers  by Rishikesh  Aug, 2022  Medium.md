---
slug: cloudflare-redirector
title: Create your own Bit.ly Service Using Cloudflare Workers
authors: rishi
tags: [Cloudflare]
date: 2022-08-22
description: Suppose you have a team document that should always be available in abc.com/constant but the document origin should keep changing and that too for free. Well if this is what you want to achieve, then I’ve got you covered.
---

Do you use Bitly or Bit.ly Service ?

Mostly in your life you would have used any one of the link shortening services such as bitly, cuttly or any other service either for sharing a google doc or some article with a long link into a short one.

But the problem with bit.ly and other services is that you can’t customise the url portion like cutt.ly/rishi-is-awesome.

Even if you can customise it, you can’t change the origin url without using their premium plans.

Suppose you have a team document that should always be available in abc.com/constant but the document origin should keep changing and that too for free. Well if this is what you want to achieve, then I’ve got you covered.

## Introducing Cloudflare Workers and KV !!

You don’t need to be an expert to use this method simply following this ten steps can give you a lifetime free url shortener and that too in your customisable domain for free.

Step 1:

Create a Cloudflare Account if you don’t already have one.

![](https://miro.medium.com/max/875/1*kFzunhhNrm8mEBB5K7gABw.png)

Cloudflare

Step 2:

After the verification of email login into your account and you can see

![](https://miro.medium.com/max/370/1*S8zSVzPN1jTFv04IgALDBg.png)

Click on Workers

It will ask you to create a subdomain for workers. You can give any name you want so feel free to get a catchy name before someone else does.

Step 3:

Click on KV and create Namespace

![](https://miro.medium.com/max/875/1*Me2KyzL5PMx7CKfhX5V7Ng.png)

Give it a easy name and click add

![](https://miro.medium.com/max/875/1*tiIbI0rbm5yJpSG9VOT-tw.png)

Step 4:

Click on Overview on left and click create a service

![](https://miro.medium.com/max/875/1*hWVShpCT3ewhFfk0UH-n4w.png)

Now you can give a service url so that ur service is accessible like url.yourdomain.workers.dev/your-link

![](https://miro.medium.com/max/875/1*se9ZgrOHjhsmV1jvDyIN7Q.png)

click on Create Service

Step 5:

Your service will be created

![](https://miro.medium.com/max/875/1*7jiZnb9v06G0Z-hFarWVHg.png)

now click on quick edit

![](https://miro.medium.com/max/875/1*_Qo_bpNZISDFGg02ez2-QA.png)

Go to this link and copy paste this code into the left pane

```
! function(e) {    var t = {};    function n(r) {        if (t[r]) return t[r].exports;        var o = t[r] = {            i: r,            l: !1,            exports: {}        };        return e[r].call(o.exports, o, o.exports, n), o.l = !0, o.exports    }    n.m = e, n.c = t, n.d = function(e, t, r) {        n.o(e, t) || Object.defineProperty(e, t, {            enumerable: !0,            get: r        })    }, n.r = function(e) {        "undefined" != typeof Symbol && Symbol.toStringTag && Object.defineProperty(e, Symbol.toStringTag, {            value: "Module"        }), Object.defineProperty(e, "__esModule", {            value: !0        })    }, n.t = function(e, t) {        if (1 & t && (e = n(e)), 8 & t) return e;        if (4 & t && "object" == typeof e && e && e.__esModule) return e;        var r = Object.create(null);        if (n.r(r), Object.defineProperty(r, "default", {                enumerable: !0,                value: e            }), 2 & t && "string" != typeof e)            for (var o in e) n.d(r, o, function(t) {                return e[t]            }.bind(null, o));        return r    }, n.n = function(e) {        var t = e && e.__esModule ? function() {            return e.default        } : function() {            return e        };        return n.d(t, "a", t), t    }, n.o = function(e, t) {        return Object.prototype.hasOwnProperty.call(e, t)    }, n.p = "", n(n.s = 0)}([function(e, t, n) {    "use strict";    Object.defineProperty(t, "__esModule", {        value: !0    });    const r = n(1);    addEventListener("fetch", e => {        const t = e.request;        "GET" === t.method ? e.respondWith((0, r.handleRequest)(t)) : e.respondWith(new Response(null, {            status: 405,            statusText: "Method Not Allowed"        }))    })}, function(e, t, n) {    "use strict";    Object.defineProperty(t, "__esModule", {        value: !0    }), t.handleRequest = void 0, t.handleRequest = async function(e) {        const t = new URL(e.url),            n = t.pathname,            r = t.host + t.pathname,            o = await REDIRECT_KV.get(r) || await REDIRECT_KV.get(n);        if (!o || "404" === o) {            const n = await REDIRECT_KV.get(t.host + "/404") || await REDIRECT_KV.get("404");            return "pass" === n ? fetch(e) : n ? Response.redirect(n, 301) : new Response("Sorry, page not found.", {                status: 404,                statusText: "page not found"            })        }        if ("pass" === o) return fetch(e);        const s = new URL(o);        Array.from(t.searchParams).forEach(([e, t]) => {            !1 === s.searchParams.has(e) && s.searchParams.set(e, t)        });        const a = s.toString();        return Response.redirect(a, 301)    }}]);
```

![](https://miro.medium.com/max/875/1*MgCAPpos5xPjndUTZpztvw.png)

Click on Save and Deploy

Step 6:

![](https://miro.medium.com/max/875/1*BxD6dKIhuuAW_nCiCSOXZA.png)

Click Settings and Click Variables

![](https://miro.medium.com/max/875/1*pnklRS1AI3lGuHfvz4LHjA.png)

Scroll Down and click on Add binding

![](https://miro.medium.com/max/875/1*4nQv00J3RkN0ayyVB2Xpew.png)

Type Variable name as

REDIRECT\_KV and select the KV you created from the drop down list and click save

Step 7:

Now click on KV again from the left pane. It will be under Workers

![](https://miro.medium.com/max/419/1*WwmjWmQU0rf9FmVJslUhkQ.png)

Click on the KV you created and type the key as your short url and the value as destination

![](https://miro.medium.com/max/875/1*NuL7S75Sa8CAYFUQpLL4ZQ.png)

For example if i want to go to google.com and my short link should be search i will type

search in key and [https://google.com](https://google.com/) in value

Click on Add entry and you are done

Step 8:

> Now you have a working Redirect Service under your **_project.domainname_**.workers.dev/**xxx**

![](https://miro.medium.com/max/875/1*8mqFURdX4rS6qEk6-jdQmA.png)

If you Directly go to your domain **_project.domainname_**.workers.dev then it will say page not found. If you type the correct short-url you made then it will be successfully redirected.

Oh Did I mention 10 steps at the beginning ?

Then other two steps are to follow me and share this article to your friends so that even they can own a private service like bittly.