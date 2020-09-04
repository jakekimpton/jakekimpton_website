---
title: How to track rage clicks in Google Tag Manager
date: 2020-09-04T15:19:04.881Z
image: /images/butler.jpg
tags:
  - GTM
  - GA
  - User Analysis
draft: true
---
```javascript
// Start Config 
let coolDownSpeed = 400 // milliseconds
let rageClickThreshold = 3 // number of clicks if succession to trigger an event
// End Config ---


let clickedElement = null;
let newClickedElement = null;
let clicks = 0;
rageClickThreshold--;

document.addEventListener("click", function (e) {
    if (clicks >= rageClickThreshold) {
        window.dataLayer = window.dataLayer || [];
        window.dataLayer.push({
            'event': 'rage-click',
            'rage-click' : {
                'innerText' : clickedElement.innerText || null,
                'classList' : clickedElement.classList || null,
                'id' : clickedElement.id || null
            }
        });
    }
    if (e == null) {
        newClickedElement = e.srcElement;
    } else {
        newClickedElement = e.target;
    }

    if (clickedElement == newClickedElement) {
        clicks = clicks + 1;
    } else {
        clicks = 0;
        clickedElement = newClickedElement;
    }
});

// Rage Click Cooldown
setInterval(function () {
    if (clicks > 0) {
        clicks = clicks - 1;
    }
}, coolDownSpeed)
```