## Objective
Catch at least one of each species of fish that live around Geese islands. When you're done, report your findings to Poinsettia McMittens.

## Solution

#### Fishes heatmap discovery

The heatmap can be discovered from the source code comments in the sea endpoint ( https://2023.holidayhackchallenge.com/sea/)
```html
<!-- <a href='fishdensityref.html'>[DEV ONLY] Fish Density Reference</a> -->
```
We can get all the heatmap of various fishes from these https://2023.holidayhackchallenge.com/sea/fishdensityref.html.
This list will also help us to indentify all the fishes we have to catch which is around 171 in total.

#### Cast and Reel Internals

When we press the "CAST" button and "REEL" button the internals works in these following ways

- There is constant message relay whenever we move the boat ```ks:8``` ```ks:4```
- When we press CAST a websocket message is triggered to send a ```cast``` request.
- When the websocket reply is sent containing the fish that takes the bait and needs to be reeled in.
```json
{
    "fish": {
        "name": "Cuckoo Bubblegum Unicornfish",
        "description": "An exotic, vibrant fish distin....",
        "food": "The Cuckoo Bubblegum Unicornfish has ....",
        "rarity": 0.11506656184792519,
        "hash": "332fc6fbe6b8fc601cdf82cc90978810"
    }
}
```


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
