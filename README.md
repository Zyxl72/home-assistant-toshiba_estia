# API Docu for Toshiba Estia Heat Pump
## Resources

- https://github.com/KaSroka/Toshiba-AC-control
- https://gist.github.com/h4de5/7f97db0f4efc265e48904d4a84dab4fb
- https://github.com/h4de5/home-assistant-toshiba_ac/issues/116

Thanks to h4de5, Martinnj and KaSroka

## PHP Code changed to Estia 
<details><summary>PHP Code</summary>
	
```php
<?php

	$username = "<USERNAME>";
	$password = "<PASSWORD>";

	/**
	 * @param string $url
	 * @param string $post
	 * @param string $token
	 * @return []
	 */
	function query($url, $post = null, $token = null) {
		$ch = curl_init();
		//set the url, number of POST vars, POST data
		curl_setopt($ch, CURLOPT_URL, $url);
		if (!empty($post)) {
			curl_setopt($ch, CURLOPT_POST, true);
			curl_setopt($ch, CURLOPT_POSTFIELDS, $post);
		}
		$header = [
			'Content-Type: application/json'
		];
		if (!empty($token)) {
			$header[] = 'Authorization: Bearer ' . $token;
		}

		curl_setopt($ch, CURLOPT_HTTPHEADER, $header);
		curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

		$result = curl_exec($ch);
		curl_close($ch);
		// echo $result . PHP_EOL;
		if (!empty($result)) {
			return json_decode($result, true);
		} else {
			echo "Error in query: " . $url . PHP_EOL;
			return false;
		}
	}

// 	$base_url = "https://toshibamobileservice.azurewebsites.net";
	// new url since 2022-07-14
	$base_url = "https://mobileapi.toshibahomeaccontrols.com";
	$login_url = "/api/Consumer/Login";
	$device_url = "/api/Estia/GetRegisteredEstiaByUniqueId";
	$mapping_url = "/api/Estia/GetConsumerEstiaMapping";
	$status_url = "/api/Estia/GetCurrentEstiaStateByUniqueDeviceId";
	$settings_url = "/api/Estia/GetConsumerProgramSettings";

  echo '//////////////////////////////////////////////////////////' . PHP_EOL;
  ///////////// LOGIN RESULT

	//The data you want to send via POST
	$fields = [
		'Username' => $username,
		'Password' => $password
	];

	// url-ify the data for the POST
	$fields_string = json_encode($fields);
	$result_object = query($base_url . $login_url, $fields_string);
	var_export($result_object) . PHP_EOL;

	// Store access token and consumerId for further calls
	$access_token = $result_object['ResObj']['access_token'];
	$consumerId = $result_object['ResObj']['consumerId'];

	echo '//////////////////////////////////////////////////////////' . PHP_EOL;
  ///////////// MAPPING RESULT
	$fields = http_build_query([
		'consumerId' => $consumerId
	]);
	$result_object = query($base_url . $mapping_url . '?' . $fields, null, $access_token);
	var_export($result_object) . PHP_EOL;
	// store first AC id
	$deviceId = $result_object['ResObj'][0]['ACList'][0]['DeviceUniqueId'];

	echo '//////////////////////////////////////////////////////////' . PHP_EOL;
  ///////////// DEVICE STATUS RESULT
	$fields = http_build_query([
		'DeviceUniqueId' => $deviceId
	]);
	$result_object = query($base_url . $status_url . '?' . $fields, null, $access_token);
  
	var_export($result_object) . PHP_EOL;

	echo '//////////////////////////////////////////////////////////' . PHP_EOL;
  ///////////// DEVICE SETTING RESULT
	$fields = http_build_query([
		'consumerId' => $consumerId
	]);
	$result_object = query($base_url . $settings_url . '?' . $fields, null, $access_token);
	var_export($result_object) . PHP_EOL;

	echo '//////////////////////////////////////////////////////////' . PHP_EOL;
	?>
```

</details>

## C

### B
#####
|Temp(°C)|20|21|22|23|24|25|26|27|28|29|30|31|32|33|34|35|36|37|38|39|40|41|42|43|44|45|46|47|48|49|50|51|52|53|54|55|56|57|58|59|60|61|62|63|64|65|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|Code|48|4a|4c|4e|50|52|54|56|58|5a|5c|5e|60|62|64|66|68|6a|6c|6e|70|72|74|76|78|7a|7c|7e|80|82|84|86|88|8a|8c|8e|90|92|94|96|98|9a|9c|9e|a0|a2|



