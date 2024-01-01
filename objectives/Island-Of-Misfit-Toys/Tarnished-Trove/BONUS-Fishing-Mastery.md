- https://2023.holidayhackchallenge.com/sea/fishdensityref.html
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
