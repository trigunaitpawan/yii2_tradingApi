# SmartAPI - PHP Client SDK

## Installation

#### Without composer
Download library from [here](https://github.com/trigunaitpawan/yii2_tradingApi/archive/refs/heads/master.zip) and use as your custom library


#### Via composer
```bash
composer require smartapi/tradinglite
```



## Usage
The simplest usage of the library would be as follows:
```
<?php

require_once __DIR__ . '/vendor/autoload.php';

$smart_api  = new \TradingLite\SmartApi(
// OPTIONAL
    //  "YOUR_ACCESS_TOKEN",
    // "YOUR_REFRESH_TOKEN");
	
$smart_api ->GenerateSession("client-code","password");
   

?>
```


## Usage for frameworks
Import package to controller
```
use TradingLite\SmartApi;
```

## Getting started with API
```php
  //in your function of controller create instance of TradingLite class

    $smart_api  = new SmartApi(
    				// OPTIONAL
			    //  "YOUR_ACCESS_TOKEN",
			    // "YOUR_REFRESH_TOKEN"
				);
    
    //Login
    $smart_api ->GenerateSession("Client-code","password");
    
    //methods
    $token = $smart_api ->GenerateToken();  
    
    $profile = $smart_api ->GetProfile();
    
    $smart_api ->LogOut(array('clientcode'=>'your client-code'));
    
    $rms = $smart_api ->GetRMS();
    
    $order = $smart_api ->PlaceOrder(array('variety' => 'NORMAL',
                                    'tradingsymbol'  =>  'JINDALSTEL-EQ',
                                    'symboltoken' => '6733',
                                    'exchange' => 'NSE',
                                    'transactiontype' => 'SELL',
                                    'ordertype' => 'LIMIT',
                                    'quantity' => '1',
                                    'producttype' => 'INTRADAY',
                                    'price' => 312.65,
                                    'squareoff' => 0,
                                    'stoploss' => 0,
                                    'duration' => 'DAY'));
                                    
     $modifyOrder = $smart_api ->ModifyOrder(array('variety' => 'NORMAL',
                                    'tradingsymbol'  =>  'JINDALSTEL-EQ',
                                    'symboltoken' => '6733',
                                    'exchange' => 'NSE',
                                    'transactiontype' => 'BUY',
                                    'ordertype' => 'LIMIT',
                                    'quantity' => '1',
                                    'producttype' => 'INTRADAY',
                                    'price' => 200,
                                    'squareoff' => 0,
                                    'stoploss' => 0,
                                    'duration' => 'DAY',
                                    'orderid' =>210312000000394));
    
    $cancelOrder = $smart_api ->CancelOrder(array('variety' => 'NORMAL',
                                      'orderid' => '210312000000394'));
                                      
    $GetOrderBook = $smart_api ->GetOrderBook();
    
    $GetTradeBook = $smart_api ->GetTradeBook();
    
    $GetHoldings =  $smart_api ->GetHoldings();
    
    $GetPosition  = $smart_api ->GetPosition();
    
    $ConvertPosition  = $smart_api ->ConvertPosition(array("exchange"=>"NSE",
                                                "oldproducttype"=>"INTRADAY",
                                                "newproducttype"=>"MARGIN",
                                                "tradingsymbol"=>"JINDALSTEL-EQ",
                                                "transactiontype"=>"SELL",
                                                "quantity"=>"1",
                                                "type"=>"DAY"));
                                                
   $CreateRule  = $smart_api ->CreateRule(array("tradingsymbol" => "SBIN-EQ", 
                                      "symboltoken" => "3045", 
                                      "exchange" => "NSE", 
                                      "producttype" => "MARGIN", 
                                      "transactiontype" => "BUY",
                                      "price" => 100000, 
                                      "qty" => 10, 
                                      "disclosedqty"=> 10, 
                                      "triggerprice" => 200000,
                                      "timeperiod" => 365));
                                       
   $ModifyRule  = $smart_api ->ModifyRule(array('id' => '1000059',
                                       "tradingsymbol" => "SBIN-EQ", 
                                      "symboltoken" => "3045", 
                                      "exchange" => "NSE", 
                                      "producttype" => "MARGIN", 
                                      "transactiontype" => "BUY",
                                      "price" => 100000, 
                                      "qty" => 20, 
                                      "disclosedqty"=> 10, 
                                      "triggerprice" => 200000,
                                      "timeperiod" => 365));
                                      
    $CancelRule = $smart_api ->CancelRule(array('symboltoken'  => '3045'
                                       'exchange'   =>   'NSE' ,
                                      'id'  => '1000059'));
                                      
    $RuleDetails = $smart_api ->RuleDetails(array('id'=>'1000059'));
    
    $RuleList = $smart_api ->RuleList(array( "status"=> [
                                      "NEW",
                                      "CANCELLED",
                                      "ACTIVE",
                                      "SENTTOEXCHANGE",
                                      "FORALL"
                                 ],
                                 "page"=> 1,
                                 "count"=> 10));
                                 
    $GetCandleData = $smart_api ->GetCandleData(array("exchange"=> "NSE",
                                             "symboltoken"=> "3045",
                                             "interval"=> "ONE_MINUTE",
                                             "fromdate"=> "2021-02-08 09:00",
                                             "todate"=> "2021-02-08 09:16"));
					     
  $GetLtpData = $smart_api ->GetLtpData(array('exchange'  => 'NSE',
	'tradingsymbol'   =>   'SBIN-EQ' ,
	'symboltoken'  => '3045'));
    
```

## Getting started with SmartAPI Websocket's

Download [socket.js](https://github.com/trigunaitpawan/yii2_tradingApi/blob/master/socket.js) from src/socket.js and add it to your assets folder

```javascript

//include script.js in you current file
<script src="YOUR_FOLDER_PATH/socket.js"></script>

//write script wherever you want live steaming
	<script type="text/javascript">
	
		var ws =new  websocket('clientCode', 'feedToken');
		
		//connect to server
    ws.connection();

      //add callback after socket connection
      ws.on('connect', connectionOpen);

      function connectionOpen()
      {   
         ws.runScript("nse_cm|2885", "mw");
          // SCRIPT: exchange|token for multi stocks use & seperator, mcx_fo|222900  ### TASK: mw|sfi|dp
      }

     //add callback method where you can manipulate socket data
    ws.on('tick', receiveTick);

    //user defined function
    function receiveTick(data) {
      
      console.log(data);
         if (data.length == 0) 
         {
             ws.close();
         }

    
    }
	</script>
```