```javascript
    // Function to check for the button and click it
    function checkAndClickButton() {
        var button = document.querySelector('.reelitin.gotone');
        if (button) {
            socket.send(`reel`);
        }
    }

    // Check and click the button when the page loads
    window.addEventListener('load', checkAndClickButton);
    setInterval(checkAndClickButton, 1000);
```
