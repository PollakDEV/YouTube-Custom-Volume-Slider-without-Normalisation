# YouTube-Custom-Volume-Slider-without-Normalisation

A lightweight userscript that adds a **custom volume slider** directly into YouTube’s video player controls. Designed to be used primarily with **YT Enhancer**, but also works with other userscript managers like **ScriptMonkey** or **Tampermonkey**.

---

## Features

- Adds a **custom volume slider** inside YouTube’s player controls.  
- Real-time synchronization: the slider updates if the video volume changes elsewhere.  
- Fully compatible with **YT Enhancer**.  
- Lightweight and easy to maintain.

---

## Installation

1. Install **YT Enhancer**.  
2. Add this script to YT Enhancer.  
3. Restart Browser — the slider will appear in the player controls.  

---

## Script
```javascript
(function() {
    'use strict';

    function getVideo() {
        return document.querySelector('video');
    }

    function getRightControls() {
        return document.querySelector('.ytp-right-controls');
    }

    function createSlider() {
        const container = document.createElement('div');
        container.style.display = 'flex';
        container.style.alignItems = 'center';
        container.style.marginLeft = '10px';

        const slider = document.createElement('input');
        slider.type = 'range';
        slider.min = 0;
        slider.max = 100;
        slider.value = 100;
        slider.style.width = '80px';
        slider.style.height = '4px';
        slider.style.cursor = 'pointer';

        slider.addEventListener('input', () => {
            const video = getVideo();
            if (video) {
                video.volume = slider.value / 100;
            }
        });

        setInterval(() => {
            const video = getVideo();
            if (video) {
                const newValue = Math.round(video.volume * 100);
                if (parseInt(slider.value) !== newValue) {
                    slider.value = newValue;
                }
            }
        }, 200);

        container.appendChild(slider);
        return container;
    }

    function addSliderToControls() {
        const controls = getRightControls();
        if (!controls) return;


        if (document.querySelector('.custom-volume-slider')) return;

        const sliderContainer = createSlider();
        sliderContainer.classList.add('custom-volume-slider');
        controls.insertBefore(sliderContainer, controls.firstChild);
    }

    const observer = new MutationObserver(addSliderToControls);
    observer.observe(document.body, { childList: true, subtree: true });
})();

```
