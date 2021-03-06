# Wargaming Public API 1.3
Basic PHP library handling Wargaming Public API. Uses namespace to get data sources so it is compatible with all sources (WoT, Blitz, WGN, WoWp and also WoWs) and all servers (EU,NA,ASIA,RU,KR). All it needs is application_id that can be obtained here https://eu.wargaming.net/developers/applications/ (for EU).

## Requirements
- PHP 7
- CURL

## Sample usage
``` php
<?php

// Include API
require_once 'api.php';

// API Instance where demo is your application_id
$api = new Wargaming\API('demo');

// Test how it works
try {
	$data = $api->get('wgn/clans/list', ['search'=>'PSQD']);
	
	// Display info about WoT Clan PSQD
	var_dump($data);
	
} catch (Exception $e) {

	die($e->getMessage());
	
}
```

## Sample ETag usage
``` php
<?php

// Include API
require_once 'api.php';

// API Instance where demo is your application_id
$api = new Wargaming\API('demo');

// Test how it works
try {
	// As a 4th param provide ETag. If tag of a clan S3AL wasn't changed method will return true. If it changed new data will be returned.
	$info = $api->get('wgn/clans/info', ['clan_id'=>'500034335','fields'=>'tag'], false, '813ac115749538da9b3b61fd4069fd44');
	
	var_dump($info);die;
	
} catch( Exception $e) {
	
	exit('Error: '.$e->getMessage());
	
}
```

## How to get ETag from API request
``` php
<?php

// Include API
require_once 'api.php';

// API Instance where demo is your application_id
$api = new Wargaming\API('demo');

// Test how it works
try {
	// Set 5th param to boolean TRUE. That way method will return array with following format: ['headers'=>[],'data'=>StdClass]
	$info = $api->get('wgn/clans/info', ['clan_id'=>'500034335','fields'=>'tag'], false, null, true);

	// Get response headers. Remember to store ETag without quotes cause $api->get() method add those when ETag is provided.
	var_dump($info['headers']['ETag']);die;
	
} catch( Exception $e) {
	
	exit('Error: '.$e->getMessage());
	
}
```

## News
### 1.3.1 - 2018-02-26
Creating composer package.

### 1.3 - 2017-10-09
Moving to PHP7, changing code to PSR.

### 1.2 - 2016-02-06
From now if CURL return an error class will throw exception with that error message. Also added support for ETag ([Documentation](https://eu.wargaming.net/developers/documentation/guide/getting-started/#etag)) and HTTP headers.

### 1.1 - 2015-08-20
Moved to CURL for requests. Speed boost shoold be from 50% - 150% cause of reusing connection. Checked on my WN8 calculation class. Regular calculation takes 0.32s

### 1.04 - 2015-08-19
Fixed few error messages.

### 1.03 - 2015-08-17
Fixed error handling for associative arrays, added new error messages.

### 1.02 - 2015-08-17
Added new parameter to `API::get()` called $assoc. If this parameter is set to true, function will return associative array instead of object/array of objects.
