## Objective

Visit Christmas Island and talk to Morcel Nougat about this great new game. Team up with another player and show Morcel how to win against Santa!

## Solution

There are couple of ways to solve this challege



## Automation

```Javascript
function godMode(){
	player.health=999;
}

function defeat_elves(){
	keysArray = Object.keys(allElves);

	for(i=0;i<keysArray.length;i++){
    		elf_id = keysArray[i]

	data = {
		"a": "e",
		"i": player.playerId,
		"eid": elf_id
	}
	ws.send(JSON.stringify(data))
	}
}

function defeat_santa(){

	data = {
		"a": "s",
		"i": player.playerId,
	}
	ws.send(JSON.stringify(data))

}


function autoPlay(){
	godMode();
	defeat_elves();
	defeat_santa();
}

const intervalId = setInterval(autoPlay, 1000);
```
