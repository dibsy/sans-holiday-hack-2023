## Objective
Catch at least one of each species of fish that live around Geese islands. When you're done, report your findings to Poinsettia McMittens.

## Solution

### Fishes heatmap discovery

The heatmap can be discovered from the source code comments in the sea endpoint ( https://2023.holidayhackchallenge.com/sea/)
```html
<!-- <a href='fishdensityref.html'>[DEV ONLY] Fish Density Reference</a> -->
```
We can get all the heatmap of various fishes from these https://2023.holidayhackchallenge.com/sea/fishdensityref.html.
This list will also help us to indentify all the fishes we have to catch ( 171 in total )
```javascript
    // Function to check for the button and click it
    function checkAndClickButton() {
        var button = document.querySelector('.reelitin.gotone');
        if (button) {
            socket.send(`reel`);
        }
    }

    setInterval(checkAndClickButton, 1000);


```
```javascript
    function checkStyleChange() {
        var button = document.querySelector('.castreel');
        
        // Check if the style display is set to none
        if (button && button.style.display === 'block') {
            socket.send(`cast`);
        }
    }

    // Check for style changes every second
    setInterval(checkStyleChange, 1000);
```
