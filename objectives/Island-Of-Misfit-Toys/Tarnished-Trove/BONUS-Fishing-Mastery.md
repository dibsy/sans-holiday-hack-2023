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
    function reelMe() {
        var button = document.querySelector('.reelitin.gotone');
        if (button) {
            socket.send(`reel`);
        }
    }

    setInterval(reelMe, 1000);


```
```javascript
    function castMe() {
        var button = document.querySelector('.castreel');        
        if (button && button.style.display === 'block') {
            socket.send(`cast`);
        }
    }

    setInterval(castMe, 1000);
```

#### Finding our fishes image
- Although most of the fishes can be caught from a single locations, we need to move around for specfic fishes.
- Some fishes might not be present at all locations and we need to use heatmap to head to the location.
- We can overlay the heatmap on the minimap or replace the minimap locally with the heatmap to get the location of the fishes.

#### Getting our fishes images
- Each fish is stored at ```https://2023.holidayhackchallenge.com/sea/assets/fish/<HASH>.png```
- We can get these hashes from playerData object that stores all the game data of the user
```javascript
for(i=0;i<playerData.fishCaught.length;i++){
    console.log("<img src=\"https://2023.holidayhackchallenge.com/sea/assets/fish/"+playerData.fishCaught[i].hash+".png\">")
}
```

### Fishes
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/53d5545920b15f6f9d26aea7b8b68070.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/a7068c3f505f77adf1c48ed85a469062.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/b451c5625b14847c4f063a4689f61c3a.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/51895db935386760b205e1b24b7ff29a.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/a4ac115509b07846bcd411d52fd55eb9.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/8b62d8a41abd57333c646b2f1742b909.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/7fdb57197885b5c0e912b7abbe498538.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/72bb67601278c9ce60da7f602512dfd3.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/b6d362180d0628e354893f33f1ca0450.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/99d01619cd2ad2863f39eae567aa373f.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/d16289d988dd46075c83fa53d9f8329f.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/6c1851136b1d71561c13e28ecc17862a.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/6926afccc8ce38c2db7613137837893e.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/e7d0e3276c6273af488a8916aee8cacf.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/4cf9e2ad9b4f3d3eb4f7dc887dcbc8ce.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/3066ec38819415ba17fe0a0e8c390620.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/b6fe2298025cd93b921abb47da5a63bb.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/3068ac153c5dcf97453bbcea202d3da3.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/560de36181ace0a02b75af9b7ca97630.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/aeb3e08ad5877cdc44410da12779e913.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/5f28e157a709063786128b5974f0a515.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/b28c8abfea53b3a4255315a3ed284e89.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/1052eecec2f21b5b38bc6bfbc73be481.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/a99e65463fef583798e3cc945f65b1fc.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/fc29aa8f64ea0e81c27d86905665a1cd.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/847dd3f33500f1ae19fa7b2116efdb7b.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/3eeaa4b9e803d827ed140709be9ab82b.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/81a63b7afe0962480376444a5c42f744.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/fa861a3409ae1d106679f43931f3f633.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/ec1c31700e4c8ac9c1fa17c84e648733.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/eac523d0c03dc33a6b924058d901666f.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/23f42ab50ec424bb33b5b13d56f1d2d9.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/7af5159b8824599a4917996515b37f82.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/068f1ad164200d6ed15e72a66c6d8705.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/245365b897e6ff179d8f8eb832c2d213.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/f0f6e0c5038bfe04e906e3d6f58b4a0d.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/9b6895e3b032f64636436d0335f4a25d.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/ce7001b878ceb1f148057c7bf1271878.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/5a7d82cea9c992601c30b29abf405c7f.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/a6cd0ccd4e664572182cde0c904472cb.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/9a6c82f538f40553a0b92c830dbefacb.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/387d5a8e1ced00c08dfabf1ad272a91c.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/b516a98ec291dfae58616b9651aabdbf.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/cd8f3a56816d1f22b6786d6377cf11d4.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/5afd6ddb4c2b40c915a11075ce7257a8.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/80eadc375055644f07f55b1178223603.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/f45b67fd696f333c6a43233ebda8debd.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/24e7a7485338b7e5d1fa93871f7c91fe.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/0bb3ae308159e86d5fd9624514886d9b.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/dc31393b6364bf563f6a2b9285ed1f9c.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/7166caae92c23fe6cf53a53ef660bb47.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/63f1f93438866daca013fd4bbe378526.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/80aae6fa768121b8ea09accc66637c98.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/3e27f3cfca88b899e6bf3906ade3935c.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/3bf31cc37c5276450dc1d96fb2344f42.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/6d73cd139e17217e335514729f248633.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/309967c5137af120508290685d903fb6.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/affd0e3fb2b0126b7c1718bdb9e4855d.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/c614775f20d04d4b7a6b1071a7895a8b.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/0cfcbfe304418a16ab96231fbbcac0d0.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/9fec63119a61622c8374ebf270fc4f97.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/5f45ff15761251871d94a3fbdced99b0.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/42f5f8efad16a2fbf1a0d838c289ff68.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/2fe83338e8c239b7d48e43a39cfdcca3.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/b6c3cfe078d854ae579b3c24039e9155.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/984192a346685a812de3654bba3f6376.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/bb29d20323c8a281079846bc0bbe7fc3.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/7d19e1bb7434e0af2d5ef6c3a039f83a.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/b68d0c39fb90ba29a29e17090014ec21.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/99a863ac9236324b36ca8c353f460739.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/32a10f230e6686b14beea1c80617706f.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/2642c99e4ecc52e15afd54a61f7450e4.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/92fd8e53dd59dbb5eb5a6199953bb617.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/3c871ab26a3aa23020c4fc588fac4aba.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/7149273621661d7208873dcc21a9d28d.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/dd2cb7afbd0403904db446efb621705c.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/bb69aef3f22c40283f0ec685ccdd27a1.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/e86b5ef389003fe13e15621f181b0a2d.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/dfa48f714d0c2f22a87592c11e492a61.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/cf99a02d799fbbb4de7734ea25cffdaa.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/3c6186ab937863c9c11a2eb59ffe8858.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/2cf8b585996ae78b23cfa07b5f28e879.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/ed90c06e2d1cbfb9bfd2d8b29290f234.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/8ab4571160906741c9b6e174a5a13211.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/fec0760431739fe6e82b31c0a88be913.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/d1e2a7b54efd54f29c8fb08068d07661.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/64ffa2c8b85ff8ebbc658259449a8e1f.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/ab2ead39fbf60c4ae264ad5897565f3e.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/72abdb37852178b300570fa8f2f68aae.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/eae1c520e4991bc8e57a90f92416ad95.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/014552a1208628bae37ccef5a81f3d8f.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/bc62d34330c443e85b9171c16998dfc1.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/76fd75a94c468bea8ae528603bbb73a1.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/1cdc07e57c4f992ca67e383fdbc3f2b6.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/2e56049d12420838c690a1fd5deda646.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/332fc6fbe6b8fc601cdf82cc90978810.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/12041d965211d36cac23d280318e627f.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/17b988f101022a63a7969a1bd827ff3a.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/a2c467f7ce5f1cd02746648dc907fae9.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/588335706118b1a714c236573cb319f9.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/f5c954a251acc59cf0dca43d13f4d148.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/bda7bf1430b637b90a9abf5099635a4c.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/334833f05a0528715871c949d4455ae2.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/0e7b4cca7125cbfa76e3716a31c55b06.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/73f6652a44c0403b34e0446a39cac263.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/541e5f07cbb0955877813b295cac1ad7.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/051ac97ebe232a08f0599ece67474e07.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/281ae69b8307c9c736efb7fce1fab0e5.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/527243cbf532900a9db5c6dda1853394.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/b34c921e6375715ae3b631d15a97af7f.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/18cd9cf11feac4fe8e6899a14b49cefc.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/af08253fcc466d1c1c61c4aa28d3fa88.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/a6197c202d7ddfa058d88ca17766cc29.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/dad300376f8d63a513a5576e634a36dc.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/a77260d1bb64b5d25be1f4c4df37a028.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/a9f651e537a296f2a199f530cd553ec9.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/cde91f580e77ccd0b817cbd3b961147b.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/d208fc91c15a0a99d258feb4f2cc46ef.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/8146dce146f5ff0cec3b7b45dadacf20.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/eb40e88b4342c1ae04800b1eb72c4e8b.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/c7c77f2ddc8e649a0ebbb3f7e3781c51.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/af6b3a320c2707d31c7d2dbc20211995.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/b6714c7a5990a28702a7b0f803165d6f.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/caf57d66e6ef17949bbcbf6adceae82e.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/3bc2339ee9ebd0dffa30113cdd888cd8.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/ffddfa9514405e748a20991e03675e11.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/fadd660d626e1bc63ea763de695f6f6a.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/c39fa0c7019de8c0fc0c85b374301ea0.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/d508ecea34137c95ca69e7c7855be439.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/c827d2e8d0d622280e6749d1c724cee2.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/dc3d4f821d77dfb6d8164f020a8c89f8.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/e56ceae87cbcec5933d42c35e9dc861b.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/96513f43167d315f0b0caa6391656833.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/53b84172e7a43df667a609573a388c01.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/edc65097074ca5c232bd656776ff687d.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/2a616fea4d6cf6d3548671ee3d9cc223.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/175b565202a3d5471560405175f89e04.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/d8e50f64a0abdd4d029790018be12101.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/bcca6833812f118bc3ddd52d53defede.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/c193eaf24c49591209a2d6fbe1f631dd.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/7be5cd5b9ac330e7814647800bc272ca.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/023d9efabbba828ca10d280cb0f1d323.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/0214359cb706795056b068fcdddd1224.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/0ecf98c4757097f4f02a222e9501eaba.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/363a51e15c2697ac761635ae8d0901cc.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/6e54887f2f60f1b36d210b06210b0aae.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/1fb98d53539e9f0522de0dff9c62a692.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/2338f915f129f5e72fdd9e9d183b86b7.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/04da98a516dfd92caa31c3f360e4149b.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/a92ad2084f7371712998f503701669df.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/4b12a1e023fd3bf611dcedf8165fa7d2.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/ce8514daf6aa61074b1fbf633a0b5e07.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/91cd4afbbe6c4d9350a2b3f94e58cc98.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/29e06391b33bb5f51d00d0348bcffd79.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/b5cab0051e7d8d70f80925325b6f649a.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/249e838fc68856f87b736cb94af24599.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/1e661def78bd97c3dd93dad4a702f9bd.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/dc409eed3e161acb6b9f89ad27895f18.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/9edc9f996f1c0aca14133687ab473d9f.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/92068b82982aef175895d58f437ad4f4.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/52a2b40fd4e4a7f9f39150325ac91856.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/5aebf3af0c4f1e939e4b10407fc2c4bb.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/0394c886a4e41c4f94aa4dacd6d0c3cf.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/808826370d1a2b7ef1221f7447b48cc2.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/d040cd5dd0fe75d8fbaa73561181b859.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/a14b934e0d7b24b8e51e941a89cbda3d.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/0af2d77fe9b9adbfaa2f14860dc6a2a6.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/d020812fcb47cfcab652ccd6a2749cf4.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/22d41edd38f7369e9e646a646945427a.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/ff4996ed41b8896b7cf7f97d73c24b29.png">
<img src="https://2023.holidayhackchallenge.com/sea/assets/fish/f71ba29843c1d46325da6e8ec821896b.png">
