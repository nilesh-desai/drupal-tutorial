# Add AWS SDK PHP library using composer.

```composer require  "aws/aws-sdk-php"```

We can create a custom service or helper class to generate AWS signature key.


## Add some classes which we are going to use.

```use Aws\Credentials\Credentials;
use Aws\Signature\SignatureV4;
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Uri;
use Symfony\Component\Serializer;
```


## Add below code which will generate AWS signature key.

```
// $bodyParams should an array of data which is passed in body param of main API.
$bodyData = Serializer::encode($bodyParams, 'json');
$options = [];
$header = [
  'Content-Type' => 'application/json',
];

// Prepare Credentials.
$credentials = new Credentials(
  $token['accesskey'],
  $token['secretKey'],
  $token['sessionToken']
);

// Build request object.
$uri = new Uri($baseUrl);
$uri = $uri->withPath($url);
$request = new Request('POST', $uri, $header, $bodyData);

// Get signatured request object.
$signature = new SignatureV4('execute-api', 'us-east-1', $options);
$signatureRequest = $signature->signRequest(
  $request,
  $credentials,
);

$httpClient = new Client();
$request = $httpClient->send($signatureRequest);
$response = $request->getBody()->getContents();
```




