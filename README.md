# SpockEx

This software is an experiment. It's purpose will be made clear later.

## Entanglement Protocol

###1. Offline communitcation

* Peer A generates a JSON message within a QR code or NDEF message for Peer B to read. The message contains a peer-unique ID, along with the peer's external IP address, a peer-unique encryption key, a peer-unique salt, and a total peers message (integer from 0-max). since much of this data is peer-unique, Peer A must record a this data associated with Peer B in a relational dataebase, and anticipate more peer-unique info yet to come from Peer B. If Peer A doesn't know it's external IP, it'll get this from Peer B later, and sends an "unknown" message for now.

   #### JSON Offline Handshake: 
   
   ```json
   {
      "request": 100,
      "peerInfo": {
         "pid": "0c41c34e-bda2-405b-ae0b-c66e41c5b2db",
         "host": "either an IP address or 'unknown'",
         "key": "d23bf636a4df498aa498d7e14ea781c0",
         "salt": {
            "string": "#d7",
            "index": 14
         },
         "testString": "a random string used to test later"
      }
   }
   ```

* Peer B generates an ID, key and salt that will only be used with Peer A. This data is peer-unique, so Peer B must record a this data associated with Peer A and Peer A's information in a relational dataebase. 
* Responds via the same ofline method with this information, as well as its IP address and total peers. If Peer B does not know its external IP address, it runs IPgetter script. IPgetter should be used rarely.

   #### JSON Offline Response: 
   
   ```json
   {
	   "response": 200,
	   "peerInfo": {
		   "pid": "af2ef074-637e-4d18-83b3-047f5daf355f",
		   "host": "123.45.67.89",
		   "key": "459c5876704649cb930de10b39bb5610",
		   "salt": {
			   "string": "#9j",
			   "index": 9
		   },
		   "testString": "a random string used to test later"
	   }
   }
   ```

   #### JSON Offline Error: 
   
   ```json
   {
      "response": 400, 
      "errorText": "A brief summary of what went wrong."
   }
   ```

###2. Online Handshake

* Upon reciept, Peer A and B run NAT punching proceedures to open that IP and port on the router. (How? Not sure yet.)
* Peer A then sends a JSON message to Peer B over IP with Peer ID and an encrypted packet using agreed upon codes, keys and salts.

   #### JSON Online Handhake: 
   
   ```json
   {
	"request": 110,
	"peerId": "0c41c34e-bda2-405b-ae0b-c66e41c5b2db",
	"payload": "encrypted string"
   }
   ```
   
   #### Payload decrypted:
   
   ```json
   {
	"pid": "af2ef074-637e-4d18-83b3-047f5daf355f",
	"testString": "the random string from offline exchange"
   }
   ```
   
   (Timeouts will result in an error message, with a "Retry" or "Cancel" option. Retry goes back to the begining of step #2. Canceling must delete all prior pending data for this peer connection.)

* Peer B confirms the identity of the IP message using previous codes, keys and salts.
* If succesful, Peer B responds with a success message, includes relevant peer-unique codes, tells Peer A his external IP, and sends it all encrypted via Peer A's key and salt. If not successful, sends error.

   #### JSON Success: 
   
   ```json
   {
	"response": 200,
	"echoIp": "Always echo back the ip of the peer",
	"request": 120,
	"peerId": "af2ef074-637e-4d18-83b3-047f5daf355f",
	"payload": "encrypted string"
   }
   ```
   
   #### Payload decrypted:
   
   ```json
   {
	"pid": "0c41c34e-bda2-405b-ae0b-c66e41c5b2db",
	"testString": "the random string from offline exchange"
   }
   ```

   #### JSON Error: 
   
   ```json
   {
      "response": 400, 
      "echoIp": "Always echo back the ip of the peer",
      "errorText": "A brief summary of what went wrong."
   }
   ```
   
   (Timeouts will result in an error message, with a "Retry" or "Cancel" option. Retry goes back to the begining of step #2. Canceling or errors must delete all prior pending data for this peer connection.)

* Peer A confirms the identity of the IP message using previous codes, keys and salts. If succesful, Peer A sends a success message. Peer A also generates a Cluster UID, and sends it to Peer B. If something isn't right, sends error.

   #### JSON Success:  
   
   ```json
   {
      "response": 200, 
      "echoIp": "Always echo back the ip of the peer",
      "request": 130,
      "paylaod": "A brief summary of what went wrong."
   }
   ```
   
   #### Payload decrypted:
   
   ```json
   {
	"clusterId": "f4c68ab-2da2-48fb-bca8-a124d299a915",
	"members": ["af2ef074-637e-4d18-83b3-047f5daf355f", "0c41c34e-bda2-405b-ae0b-c66e41c5b2db"]
   }
   ```

   #### JSON Error: 
   
   ```json
   {
      "response": 400, 
      "echoIp": "Always echo back the ip of the peer",
      "errorText": "A brief summary of what went wrong."
   }
   ```
   
   (Timeouts will result in an error message, with a "Retry" or "Cancel" option. Retry goes back to the begining of step #2. Canceling or errors must delete all prior pending data for this peer connection.)

* Peer A and B are now trusted peers and a new peer cluster has been generated between them. Peer B accepts with a success code.

   #### JSON Success: 
   
   ```json
   {
      "response": 200, 
      "echoIp": "Always echo back the ip of the peer"
   }
   ```

###3. Publish IP changes to peers
