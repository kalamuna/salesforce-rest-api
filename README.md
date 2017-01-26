# salesforce-rest-api
A simple PHP client for the Salesforce REST API

## Installation

Add into composer.json:
```
    "repositories": [{
        "type": "vcs",
        "url": "https://github.com/kalamuna/salesforce-rest-api"
    }],

"require" : {
  "kalamuna/salesforce-rest-api": "dev-master"
}
```

## Usage

Initialize the `Salesforce\Client` class, call the APIs you want.

```php
use Gmo\Salesforce;
use Gmo\Salesforce\Exception;
use Guzzle\Http;

$authentication = new Salesforce\Authentication\PasswordAuthentication(
	"ClientId",
	"ClientSecret",
	"Username",
	"Password",
	"SecurityToken",
	new Http\Client()
);
$salesforce = new Salesforce\Client($authentication, new Http\Client(), "na5");

try {
	$contactQueryResults = $salesforce->query("SELECT AccountId, LastName
		FROM Contact
		WHERE FirstName = ?",
		array('Alice')
	);
	foreach($contactQueryResults as $queryResult) {
		print_r($queryResult);  // The output of a single record from the query API JSON, converted to associative array
	}

    $contactQueryResults2 = $salesforce->query("SELECT AccountId, LastName
        FROM Contact
        WHERE FirstName = :firstName",
        array('firstName' => 'Bob')
    );
    foreach($contactQueryResults2 as $queryResult) {
        print_r($queryResult);  // The output of a single record from the query API JSON, converted to associative array
    }

} catch(Exception\SalesforceNoResults $e) {
	// Do something when you have no results from your query
} catch(Exception\Salesforce $e) {
	// Error handling
}
```


