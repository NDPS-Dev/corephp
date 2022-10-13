# atomlite/corephp
[![Build Status](https://travis-ci.org)](https://travis-ci.org/joemccann/dillinger)

Official CorePHP library for NTT DATA Payment Service.

## Prerequisites
- A minimum of PHP 7.3 upto 8.1

## Installation
- If your project using composer, run the below command 
    
    ```sh
    composer require ndps/codeigniter
    ```

- If you are not using composer, download the latest release from the releases section. You should download the corephp.zip file. And place in vendor folder.


## How To Use It

- To handle the request use below function which will provide the request URL.

    ```sh
        public function payment()
         {
          include_once APPPATH . 'vendor/autoload.php';
          $transactionRequest = new \NDPS\TransactionRequest();

          /* Add your return URL */
          $ru = "http://localhost:8081/Package/CI/response";

          /*
          *Setting all values here
          */
          $transactionRequest->setLogin('192');
          $transactionRequest->setPassword("Test@123");
          $transactionRequest->setProductId("NSE");
          $transactionRequest->setAmount('50.55');
          $transactionRequest->setTransactionCurrency("INR");
          $transactionRequest->setTransactionAmount('50.55');
          $transactionRequest->setReturnUrl($ru);
          $transactionRequest->setClientCode('NAVIN');
          $transactionRequest->setTransactionId('0010');
          $transactionRequest->setCustomerName("Test Name");
          $transactionRequest->setCustomerEmailId("test@test.com");
          $transactionRequest->setCustomerMobile("9999999999");
          $transactionRequest->setCustomerBillingAddress("Mumbai");
          $transactionRequest->setCustomerAccount("639827");
          $transactionRequest->setReqHashKey("KEY123657234");
          $transactionRequest->seturl("https://paynetzuat.atomtech.in/paynetz/epi/fts");
          $transactionRequest->setRequestEncypritonKey("8E41C78439831010F81F61C344B7BFC7");
          $transactionRequest->setSalt("8E41C78439831010F81F61C344B7BFC7");

          $url = $transactionRequest->getPGUrl();
          header("Location: $url");
         }
    ```
    
- To handle the response use below function which will return the final response array.

    ```sh
        public function response()
         {
          include_once APPPATH . 'vendor/autoload.php';
          $transactionResponse = new \NDPS\TransactionResponse();

          /*
          **Enter the keys provided by NDPS
          */
          $transactionResponse->setRespHashKey("KEYRESP123657234");
          $transactionResponse->setResponseEncypritonKey("8E41C78439831010F81F61C344B7BFC7");
          $transactionResponse->setSalt("8E41C78439831010F81F61C344B7BFC7");
          $arrayofdata = $transactionResponse->decryptResponseIntoArray($_POST['encdata']);


          /*
          **Signature Verification for response and reponse verification
          */
          $verification = $transactionResponse->validateResponse($arrayofdata, "KEYRESP123657234");		
          if($verification){
            // final logic
            if($arrayofdata["f_code"] == "Ok"){
              echo "Transaction successful!";
            }
            elseif($arrayofdata["f_code"] == "C"){ 
              echo "Transaction Cancelled!";	
            }
            else{
              echo "Transaction Failed!";	
            }	  
          }
          else{
            echo "Transaction Failed!";
          }

           echo "<br><br>Response Array:<br>";
           print_r($arrayofdata);

        }   
    ```
