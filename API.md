## Liberty.Finance NFT API
--------
### 1.Pixel销售页面信息（Wayne）
> 1.1 剩余和已销售信息 + Key Metrics信息  

从服务端获取数据为：
```json
{
    "pixelRemain":800000,
    "pixelSold":200000,
    "tokenName":"PIXEL",
    "blockchainNetwork":"Binance Smart Chain",
    "price":"1.0",
    "totalSupply":1000000,
    "totalRaise":1000000,
    "maxAllocation":1000,
    "tokenSupport":"BUSD",
    "endTime":1623646870884
}
```

> 1.2购买Pixel合约接口  

输入参数：fromAddress,pixel数量
需要知道合约地址，及其合约方法

### 2.创建NFT接口（尔衡/浩洋）
> 2.1上传图片到ipfs接口  

参考：https://pinata.cloud/documentation#PinFileToIPFS  

image上传成功，得到图片的url

> 2.2上传NFT json数据到ipfs接口  

参考：https://pinata.cloud/documentation#PinJSONToIPFS  

upload json data:
```json
{
    "type":"scenery",
    "title":"Title here for nFT",
    "url":"https://www.google.com",
    "introduction":"The global leader connecting brands with video games, apps, and VR/AR. Epik brings to life unique collaborations inside of digital platforms delivering an experience that users love. ",
    "imageUrl":"https://gateway.pinata.cloud/ipfs/QmXQt3AGb2QUzVTGLvXfeg7WJN13GGqiUjM3zL1WvUs3UL",
    "imageSketchUrl":"https://gateway.pinata.cloud/ipfs/QmXQt3AGb2QUzVTGLvXfeg7WJN13GGqiUjM3zL1WvUs3UL"
}
```
返回值：
https://gateway.pinata.cloud/ipfs/QmXQt3AGb2QUzVTGLvXfeg7WJN13GGqiUjM3zL1WvUs3UL


> 2.3调用合约方法创建nft

测试网合约地址: https://ropsten.etherscan.io/address/0x2BaF539bC0916D600bC314E302A926ff53F2Af64#code
方法: mint(address to, string memory_tokenURI, uint256 index, uint256 startX, uint256 startY, uint256 xLength, uint256 yLength) 参数:
methodId = 0xfb8e04c8

```javaScript
params: [
  {
    from: '0xb60e8dd61c5d32be8058bb8eb970870f07233155',//钱包地址
    to: '0x2baf539bc0916d600bc314e302a926ff53f2af64',//合约地址
    data: '0xfb8e04c8.............'//参考http://www.jouypub.com/2018/1292c65cfbe128f290fb336d930d3bca/
  },'latest'
];

ethereum
  .request({
    method: 'eth_call',
    params,
  })
  .then((result) => {
    // The result varies by by RPC method.
    // For example, this method will return a transaction hash hexadecimal string on success.
  })
  .catch((error) => {
    // If the request fails, the Promise will reject with an error.
  });

```

调用合约方法参数包含：
```json
  {
        "memory_tokenURI":"https://gateway.pinata.cloud/ipfs/QmXQt3AGb2QUzVTGLvXfeg7WJN13GGqiUjM3zL1WvUs3UL",
        "index": 0,
        "startX":100,
        "startY":100,
        "xLength":200,
        "yLength":100
    }
```


### 3.编辑自己的NFT信息（尔衡/浩洋）

> 3.1上传图片到ipfs接口 (同2.1)

> 3.2上传NFT json数据到ipfs接口 (同2.2)

> 3.3调用合约方法编辑nft（x,y,w,h）这4个值是不可以变的

测试网合约地址:https://ropsten.etherscan.io/address/0x2BaF539bC0916D600bC314E302A926ff53F2Af64#code
方法: setTokenURI(uint256 tokenId, string memory tokenURI, bool blur) blur传flase
methodId = 0xda8438ac

```javaScript
params: [
  {
    from: '0xb60e8dd61c5d32be8058bb8eb970870f07233155',//钱包地址
    to: '0x2baf539bc0916d600bc314e302a926ff53f2af64',//合约地址
    data: '0xda8438ac.............'//参考http://www.jouypub.com/2018/1292c65cfbe128f290fb336d930d3bca/
  },'latest'
];

ethereum
  .request({
    method: 'eth_call',
    params,
  })
  .then((result) => {
    // The result varies by by RPC method.
    // For example, this method will return a transaction hash hexadecimal string on success.
  })
  .catch((error) => {
    // If the request fails, the Promise will reject with an error.
  });

```

调用合约方法参数包含：
```json
  {
       "memory_tokenURI":"https://gateway.pinata.cloud/ipfs/QmXQt3AGb2QUzVTGLvXfeg7WJN13GGqiUjM3zL1WvUs3UL",
       "index": 552
  }
```


### 4.获取全部NFT信息接口（尔衡/浩洋）

> 4.1通过合约中的方法获取所有的NFT信息 ***(价格信息怎么获取？？？？？？？)*** ==>参看（10.1 第2条）

访问Subgraph:  https://api.thegraph.com/subgraphs/name/erhenglu/libertynft
Example Query:
```graphql
{
  canvasNFTs{
    id
    tokenId
    index
    startX
    startY
    xLength
	yLength
	createTime
	updateTime
	blur
	govCounter
	unsafe
	url
	owner
  }
}
```

输出：
```json
[
    {
        "id":"0x01",
        "tokenId":"0x017889",
        "index":"0x122",
        "startX":100,
        "startY":100,
        "xLength":200,
        "yLength":100,
        "createTime":1623251669017,
        "updateTime":1623253668161,
        "blur":false,
        "govCounter":10,
        "unsafe": true,
        "url":"https://gateway.pinata.cloud/ipfs/QmXQt3AGb2QUzVTGLvXfeg7WJN13GGqiUjM3zL1WvUs3UL",
        "owner":"0x8073dfe92b13efb94f187537008e47fda5215262"
    }
]
```

> 4.2通过nft.url获取nft信息json数据
```json
{
    "type":"scenery",
    "title":"Title here for nFT",
    "url":"https://www.google.com",
    "introduction":"The global leader connecting brands with video games, apps, and VR/AR. Epik brings to life unique collaborations inside of digital platforms delivering an experience that users love. ",
    "imageUrl":"https://gateway.pinata.cloud/ipfs/QmXQt3AGb2QUzVTGLvXfeg7WJN13GGqiUjM3zL1WvUs3UL",
    "imageSketchUrl":"https://gateway.pinata.cloud/ipfs/QmXQt3AGb2QUzVTGLvXfeg7WJN13GGqiUjM3zL1WvUs3UL"
}
```

> 4.3通过json中的图片url，获取图片



### 5.购买NFT（尔衡/浩洋）
输入参数：nft.id,fromAddress,合约地址

测试网合约地址:  https://ropsten.etherscan.io/address/0x941CbE144eE720Cf99A37ab2eFD738148D517685#code
购买NFT 方法：buyToken(uint256 _tokenId)
methodId = 0x2d296bf1

```javaScript
params: [
  {
    from: '0xb60e8dd61c5d32be8058bb8eb970870f07233155',//钱包地址
    to: '0x941cbe144ee720cf99a37ab2efd738148d517685',//合约地址
    data: '0x2d296bf1.............'//参考http://www.jouypub.com/2018/1292c65cfbe128f290fb336d930d3bca/
  },'latest'
];

ethereum
  .request({
    method: 'eth_call',
    params,
  })
  .then((result) => {
    // The result varies by by RPC method.
    // For example, this method will return a transaction hash hexadecimal string on success.
  })
  .catch((error) => {
    // If the request fails, the Promise will reject with an error.
  });

```


卖NFT（设置价格,挂单）方法: readyToSellToken(uint256 _tokenId, uint256 _price)
methodId = 0x523a57cf


取消卖单 方法: CancelSellToken(uint256 _tokenId)
methodId = 0x9a9d9a5a


### 6.NFT市场，可以根据类型筛选（尔衡/浩洋）
> 6.1通过合约中的方法获取所有的NFT信息（同4.1）

> 6.2通过nft.url获取json数据（同4.2） 然后根据json中的类型筛选NFT

> 6.3通过json中的图片url，获取图片（同4.3）



### 7.查询地址的NFT信息（尔衡/浩洋）
> 7.1通过合约中的方法获取address下的所有NFT
访问Subgraph:  https://api.thegraph.com/subgraphs/name/erhenglu/libertynft
Example Query:
```graphql
{
  canvasNFTs(where: { owner: "0x799621c508498bb0a6482b6596a3a2e908bcbbba" }){
    id
    tokenId
    index
    startX
    startY
    xLength
	yLength
	createTime
	updateTime
	blur
	govCounter
	unsafe
	url
	owner
  }
}
```
输出：
```json
[
    {
        "id":"0x01",
        "tokenId":"0x017889",
        "index":"0x122",
        "startX":100,
        "startY":100,
        "xLength":200,
        "yLength":100,
        "createTime":1623251669017,
        "updateTime":1623253668161,
        "blur":false,
        "govCounter":10,
        "unsafe": true,
        "url":"https://gateway.pinata.cloud/ipfs/QmXQt3AGb2QUzVTGLvXfeg7WJN13GGqiUjM3zL1WvUs3UL",
        "owner":"0x8073dfe92b13efb94f187537008e47fda5215262"
    }
]
```


> 7.2通过nft.url获取json数据（同4.2）

> 7.3通过json中的图片url，获取图片（同4.3）




### 8.canvas余额，pixel余额（Wayne）



### 9.挖矿接口（Wayne）
> 9.1获取当前矿池apy  
从服务端获取数据：
```json
{
    "currentCANVASPrice":"1.0",
    "totalValueLocked":"123456789",
    "circulatingCANVAS":"100000.0",
    "pools":[
        {
            "poolName":"CANVAS-BNB POOL",
            "iconUrl":"https://xxxxx.com/icon/bnbPool.jpg",
            "introduction":"We encourge hodlers to stake their LP token to earn more CANVAS.Simply add CANVAS-BNB LP via Pancakeswap",
            "apr":"600.0",
            "tvl":"310006000",
            "stakedBalance":"1000.0",
            "earnedCANVAS":"2.0"
        },
        {
            "poolName":"CANVAS-BUSD POOL",
            "iconUrl":"https://xxxxx.com/icon/BUSDPool.jpg",
            "introduction":"We encourge hodlers to stake their LP token to earn more CANVAS.Simply add CANVAS-BUSD LP via Pancakeswap",
            "apr":"600.0",
            "tvl":"385006000",
            "stakedBalance":"1000.0",
            "earnedCANVAS":"2.0"
        },
        {
            "poolName":"CANVAS POOL",
            "iconUrl":"https://xxxxx.com/icon/BUSDPool.jpg",
            "introduction":"We encourge hodlers to stake their LP token to earn more CANVAS.Simply add CANVAS LP via Pancakeswap",
            "apr":"510.0",
            "tvl":"39610600",
            "stakedBalance":"1000.0",
            "earnedCANVAS":"2.0"
        }
    ]
}
```

> 9.2抵押代币

> 9.3提取抵押代币





### 10.获取nft.id的详情，包括历史交易信息（尔衡/浩洋）
> 10.1 通过ID，获取NFT链上数据详情

分两个subgraph query
1）、NFT的属性查询
访问Subgraph:  https://thegraph.com/subgraphs/name/erhenglu/libertynft
Example Query:
```graphql
{
  canvasNFTs(where: { tokenId: 2}){
    id
    tokenId
    index
    startX
    startY
    xLength
    yLength
	createTime
	updateTime
	blur
	govCounter
	unsafe
	url
	owner
  }
}
```
返回结果：
```json
{
    "id":"0x01",
    "tokenId":"0x017889",
    "index":"0x122",
    "startX":100,
    "startY":100,
    "xLength":200,
    "yLength":100,
    "createTime":1623251669017,
    "updateTime":1623253668161,
    "blur":false,
    "govCounter":10,
    "unsafe": true,
    "url":"https://gateway.pinata.cloud/ipfs/QmXQt3AGb2QUzVTGLvXfeg7WJN13GGqiUjM3zL1WvUs3UL",
    "owner":"0x8073dfe92b13efb94f187537008e47fda5215262"
}
```


2）、NFT价格和历史记录查询 
访问Subgraph: https://thegraph.com/explorer/subgraph/erhenglu/libertymarket
价格 Example query:
```graphql
{
  canvasNFTs(where: { tokenId: 2}){
    id
    tokenId
    price
    seller
    onSale 
  }
}
```
返回结果：
```json
{
    "id":"0x01",
    "tokenId":"0x017889",
    "price": "0x455255",
    "seller":"0x8073dfe92b13efb94f187537008e47fda5215262",
    "onSale":54566
}
```
onSale=true说明正在挂单售卖
onSale=false说明这个NFT没有挂单



3）、交易记录 Example query:
```graphql
{
  tradeHistories(where: { tokenId: 2}){
    id
    tokenId
    price
    seller
    buyer
    fee
    blockId
    time
  }
}
```
返回结果：
```json
[
    {
        "id":"0x01",
        "tokenId":"0x017889",
        "price": "0x455255",
        "seller":"0x8073dfe92b13efb94f187537008e47fda5215262",
        "buyer":"0x799621c508498bb0a6482b6596a3a2e908bcbbba",
        "fee":788,
        "blockId":"0x4855755",
        "time": 1623253668161
    }
]
```


> 10.2通过url获取NFT信息json
```json
{
    "type":"scenery",
    "title":"Title here for nFT",
    "url":"https://www.google.com",
    "introduction":"The global leader connecting brands with video games, apps, and VR/AR. Epik brings to life unique collaborations inside of digital platforms delivering an experience that users love. ",
    "imageUrl":"https://gateway.pinata.cloud/ipfs/QmXQt3AGb2QUzVTGLvXfeg7WJN13GGqiUjM3zL1WvUs3UL",
    "imageSketchUrl":"https://gateway.pinata.cloud/ipfs/QmXQt3AGb2QUzVTGLvXfeg7WJN13GGqiUjM3zL1WvUs3UL"
}
```


### 11.用户投票要求隐藏图片显示


### 12.用户的favorite（Wayne）
>  12.1市场中favorite数据获取  

>  12.2用户地址的favorite数据获取  







