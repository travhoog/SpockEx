### SpockEx

This software is an experiment. It's purpose will be made clear later.

## Entanglement Protocol

###1. Offline communitcation

* Peer A generates a JSON message within a QR code or NDEF message for Peer B to read. The message contains a peer-unique ID, along with the peer's external IP address, a peer-unique encryption key, a peer-unique salt, and a total peers message (integer from 0-max). since much of this data is peer-unique, Peer A must record a this data associated with Peer B in a relational dataebase, and anticipate more peer-unique info yet to come from Peer B. If Peer A doesn't know it's external IP, it'll get this from Peer B later, and sends an "unknown" message for now.

   JSON Offline Handshake: `{}`

* Peer B generates an ID, key and salt that will only be used with Peer A. This data is peer-unique, so Peer B must record a this data associated with Peer A and Peer A's information in a relational dataebase. 
* Responds via the same ofline method with this information, as well as its IP address and total peers. If Peer B does not know its external IP address, it runs IPgetter script. IPgetter should be used rarely.

   JSON Offline Response: `{}`

###2. Online Handshake

* Upon reciept, Peer A and B run NAT punching proceedures to open that IP and port on the router. (How? Not sure yet.)
* Peer A then sends a JSON message to Peer B over IP with Peer ID and an encrypted packet using agreed upon codes, keys and salts.

   JSON Online Handhake: `{}`
   
   (Timeouts will result in an error message, with a "Retry" or "Cancel" option. Retry goes back to the begining of step #2. Canceling must delete all prior pending data for this peer connection.)

* Peer B confirms the identity of the IP message using previous codes, keys and salts.
* If succesful, Peer B responds with a success message, includes relevant peer-unique codes, tells Peer A his external IP, and sends it all encrypted via Peer A's key and salt. If not successful, sends error.

   JSON Success: `{}`

   JSON Error: `{}`
   
   (Timeouts will result in an error message, with a "Retry" or "Cancel" option. Retry goes back to the begining of step #2. Canceling or errors must delete all prior pending data for this peer connection.)

* Peer A confirms the identity of the IP message using previous codes, keys and salts. If succesful, Peer A sends a success message. Peer A also generates a Cluster UID, and sends it to Peer B. If something isn't right, sends error.

   JSON Success: `{}`

   JSON Error: `{}`
   
   (Timeouts will result in an error message, with a "Retry" or "Cancel" option. Retry goes back to the begining of step #2. Canceling or errors must delete all prior pending data for this peer connection.)

* Peer A and B are now trusted peers and a new peer cluster has been generated between them. Peer B accepts with a success code.

   JSON Success: `{}`

###3. Publish IP changes to peers

#### Scenario B: One New Peer Joins Existing Cluster

#### Scenario C: Two Peers From Separate Clusters Become Bridges for the Two Clusters..
