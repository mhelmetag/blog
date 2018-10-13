---
title: "CSS Trickery and Element Tab Ninjas"
date: 2018-09-10T20:00:00-07:00
draft: false
---

## Surf Forecasts and a History of Hacking

I've been fiddling around with a prominent surf forecast website for awhile now (especially around their API). I mostly try circumnavigating blockers that prevent user without premium (a subscription service) from seeing things like extended forecasts or HD surfcams.

Just to note, I do pay for premium and would recommend it to any surfers out there (not that this is an ad for the site or anything).

## Security Through Obscurity

Normally, a surf forecast for a specific region looks like this:

![normal ventura forecast](https://s3-us-west-2.amazonaws.com/maxworld-images/normal-ventura-forecast.png)

Basically this page is saying you *could* see the next 17 days of forecast. But, without premium, you can only see the first two.

So... notice how there are green and blue rectangles (denoting the overall surf conditions for the AM and PM of the day) underneath this ad mask?

![ventura forecast mask focus](https://s3-us-west-2.amazonaws.com/maxworld-images/ventura-forecast-mask-focus.png)

A little weird, huh? But it can't be using actual data, right? That'd be dumb. This *must* be boilerplate/filler data that's used to pad the forecast table.

Maybe if we just hide that ad mask, we can see the underlying data (I just use the Chrome "Hide element" option n the Elements inspector)?

![hide ad mask](https://s3-us-west-2.amazonaws.com/maxworld-images/hide-ad-mask.png)

Better... but the data still looks blurred... there's definitely real data under there but how do we get it?

![blur css class](https://s3-us-west-2.amazonaws.com/maxworld-images/blur-css-class.png)

And luckily, looking at one of the forecast columns, we can see the some classes on one of the `div`s: `sl-condition-day-summary__body--blurred`. Seems like something we should update to be `sl-condition-day-summary__body`. That would probably remove the blur.

![blur css class removed](https://s3-us-west-2.amazonaws.com/maxworld-images/blur-css-class-removed.png)

Aha! This trick allows us to see data for up to six days!

## Chrome Extensions for Automated CSS Trickery

So we've found that a few simple CSS classes and an ad mask are all that's between us and three times the surf data. How do we automate this hack though? I sure don't want to update these classes and hide the ad mask every time I want to see a surf forecast.

For this, I created a Chrome extension called [ForecastGenie](https://github.com/mhelmetag/forecastgenie).

It's fairly simple... the timing is actually the hardest/hacky-ist part. Basically, we have to only hide the ad mask and switch some CSS classes once the forecast is loaded. To solve that, I just used a simple `setTimeout` but... there's probably a better, more surefire way to accomplish this.

I found that there are five CSS classes that blur each section of a forecast (condition, swell, wind, tide and weather). The extension loops through each CSS class, adding the unblurred version of the CSS class and removing the blurred version.

The end result looks like this...

![forecastgenie](https://s3-us-west-2.amazonaws.com/maxworld-images/forecastgenie.gif)

Just a cautionary tale that masks don't actually block information or keep a user from doing or seeing what they shouldn't.

Also a testament of how awesome Chrome extensions are!
