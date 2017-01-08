### SpockEx

This software is an experiment. It's purpose will be made clear later.

## Entanglement Protocol

###1. Offline communitcation

####Scenario A: Two Entirely New Peers

* Peer A generates a JSON message within a QR code or NDEF message for Peer B to read. The message contains an peer-unique ID, along with an external IP address, a peer-unique encryption key, a peer-unique salt, and a total peers message of "0".

   JSON Offline Handshake: {}

* Peer B, not knowing his external IP address either, runs IPgetter script. Responds via the same ofline method with similar JSON content, along with IP address from IPgetter.

   JSON Offline Response: {}

* Upon reciept, Peer A and B run NAT punching proceedures to open that IP and port on the router. (???)
* Peer A then sends a JSON message to Peer B over IP with Peer ID and an encrypted packet using agreed upon codes, keys and salts.

   JSON Online Handhake: {}

* Peer B confirms the identity of the IP message using previous codes, keys and salts.
* If succesful, Peer B responds with a success message, includes relevant peer-unique codes, tells Peer A his external IP, and sends it all encrypted via Peer A's key and salt. If not successful, sends error.

   JSON Success: {}

   JSON Error: {}

* Peer A confirms the identity of the IP message using previous codes, keys and salts.
* If succesful, Peer A sends a success message. Peer A also generates a Cluster UID, and sends it to Peer B. If something isn't right, sends error.

   JSON Success: {}

   JSON Error: {}

* Peer A and B are now trusted peers. 
* Peer B accepts with a success code.

   JSON Success: {}

#### Scenario B: One New Peer Joins Existing Cluster

#### Scenario C: Two Peers From Separate Clusters Become Bridges for the Two Clusters..
