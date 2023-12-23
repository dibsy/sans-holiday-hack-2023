## Objective
Help Garland Candlesticks on the Island of Misfit Toys get back into his luggage by finding the correct position for all four dials

## Solution
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
