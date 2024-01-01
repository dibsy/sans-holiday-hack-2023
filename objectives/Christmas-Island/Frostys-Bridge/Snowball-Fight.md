## Objective

Visit Christmas Island and talk to Morcel Nougat about this great new game. Team up with another player and show Morcel how to win against Santa!

## Solution

There are couple of ways to solve this challege

#### Solution 1

- The game url to start a new games starts with the following url ```https://hhc23-snowball.holidayhackchallenge.com/room/?username=dibsyhex&roomId=8a1dcd74&roomType=public&gameType=co-op&id=6919e241-4bdb-4166-ad77-59ef1da4d19f&dna=...&singlePlayer=false```
- We can change the parameter ```singlePlayer=false``` to ```singlePlayer=true```
- This will spawn a Elf the Dwarf and together we can defeat Santa to win the game.
- We will make the players health to a higher value ```player.health=999;``` using dev console.

#### Automation

- In a multi player mode we can mimic the attack of the 

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
