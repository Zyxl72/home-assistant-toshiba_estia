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
|:---:|:---:|
|a|b|
|1|2|
