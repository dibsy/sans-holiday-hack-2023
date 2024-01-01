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

### Cast and Reel Internals


#### Websockets
When we press the "CAST" button and "REEL" button the internals works in these following ways

- The socket uses a ```socket``` object to send and receive messages.
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

- When we click REEL-IN a websocket message is triggerd ```reel``` to catch the fish

#### UI Changes Observation
- The default "Cast Line" buttton has html ```<button class="castreel" style="display: block;">Cast Line</button>```
- When we click the button the html changes to ```<button class="castreel" style="display: none;">Cast Line</button>```
- The default "Reel It In" buttton has html ```<button class="reelitin" style="display: none;">Reel it in</button>```
- When we click the button the html changes to ```<button class="reelitin gotone" style="display: block;">Reel it in!</button>```


#### Our Auto Fishing Bot

- The code has 2 segments : One to Cast and another to REEL
- When the "Cast Line" is not clicked, ( as from state 2 ), cast the fishing rod"
- When the "Reel It in" is lit in red, reel the fishing rod ( as from state 4 )
  
```javascript
    function reel-me() {
        var button = document.querySelector('.reelitin.gotone');
        if (button) {
            socket.send(`reel`);
        }
    }

    setInterval(reel-me, 1000);


```
```javascript
    function cast-me() {
        var button = document.querySelector('.castreel');
        
        if (button && button.style.display === 'block') {
            socket.send(`cast`);
        }
    }

    setInterval(cast-me, 1000);
```
