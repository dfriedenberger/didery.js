#############
batchPutBlobs
#############
This function uses fetch to hit the PUT history endpoint of multiple didery servers. The PUT blobs endpoint updates
encrypted key blobs. The PUT endpoint should only be used to update the current key after key rotation and revocation
events. Data put to the servers will be verified against signatures. If any discrepancies are found the operation will
fail. This function is asynchronous.

Parameters
==========
putBlobs has four required parameters. First is a signature string that will be placed in the required signature
header. This string should be of the format:
::
  'signer="AeYbsHot0pmdWAcgTo5sD8iAuSQAfnH5U6wiIGpVNJQQoYKBYrPPxAoIc1i5SHCIDS8KFFgf8i0tDq8XGizaCg==";'
or:
::
  'signer="AeYbsHot0pmdWAcgTo5sD8iAuSQAfnH5U6wiIGpVNJQQoYKBYrPPxAoIc1i5SHCIDS8KFFgf8i0tDq8XGizaCg==";'kind="ed25519"; ...'
Second is an object containing the data to be posted. This data object should be of the format:
::
  {
      "blob": "AeYbsHot0pmdWAcgTo5sD8iAuSQAfnH5U6wiIGpVNJQQoYKBYrPPxAoIc1i5SHCIDS8KFFgf8i0tDq8XGizaCgo9yjuKHHNJZFi0QD9K6Vpt6fP0XgXlj8z_4D-7s3CcYmuoWAh6NVtYaf_GWw_2sCrHBAA2mAEsml3thLmu50Dw",
      "changed": "2000-01-01T00:00:00+00:00",
      "id": "did:dad:Qt27fThWoNZsa88VrTkep6H-4HA8tr54sHON1vWl6FE="
  }
Third is a did parameter that takes a DID string. Fourth is a urls parameter that takes an array of URL strings.

Return
======
batchPutBlobs returns a promise that when fulfilled returns the servers' responses to the fetch operations.

Example
=======
.. code-block:: javascript

   const didery = require('didery');

   let urls = [
        "http://myDideryServer.com",
        "http://anotherDideryServer.com",
        "http://oneMoreServer.com"
   ];
   let signature = 'signer="AeYbsHot0pmdWAcgTo5sD8iAuSQAfnH5U6wiIGpVNJQQoYKBYrPPxAoIc1i5SHCIDS8KFFgf8i0tDq8XGizaCg==";';
   let data = {
        "blob": "AeYbsHot0pmdWAcgTo5sD8iAuSQAfnH5U6wiIGpVNJQQoYKBYrPPxAoIc1i5SHCIDS8KFFgf8i0tDq8XGizaCgo9yjuKHHNJZFi0QD9K6Vpt6fP0XgXlj8z_4D-7s3CcYmuoWAh6NVtYaf_GWw_2sCrHBAA2mAEsml3thLmu50Dw",
        "changed": "2000-01-01T00:00:00+00:00",
        "id": "did:dad:Qt27fThWoNZsa88VrTkep6H-4HA8tr54sHON1vWl6FE="
   };
   didery.batchPutBlobs(signature, data, urls).then(function (response) {
        // Do something with response
   }).catch(function (error) {
       console.error(error);
   });