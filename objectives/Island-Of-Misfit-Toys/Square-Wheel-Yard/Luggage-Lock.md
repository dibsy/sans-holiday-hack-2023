## Objective
Help Garland Candlesticks on the Island of Misfit Toys get back into his luggage by finding the correct position for all four dials

## Solution

#### Unintended Automated Way 
- Every validation is done using web sockets.
- There can be 9000 valid combinations. We bruteforce our way in by sending all combinations by socket messages
 
```Javascript
function addwhitespace(i){
	t = (""+i).length;
	z="";
	for(x=t;x<4;x++){
    		z+="0";
	}
	return z+i
}

function unlock2(i){
	socket.emit('message', { "Type": "Open", "Combo": i });
}

for(i=0;i<10000;i++){
	p = addwhitespace(i)
	unlock2(p)
}
```

![image](../../../assets/293613670-c1763a86-054b-4bb0-8fdf-a6e2a6b48e6b.png)
