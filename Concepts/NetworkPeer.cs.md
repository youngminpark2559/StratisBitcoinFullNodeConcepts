**Folder StratisBitcoinFullNode\src\Stratis.Bitcoin\P2P\Peer\ **

**File StratisBitcoinFullNode\src\Stratis.Bitcoin\P2P\Peer\NetworkPeer.cs **



**File StratisEdited\src\Stratis.Bitcoin\P2P\PeerConnector.cs **

---


This interface represents Contract for "PeerConnector.
    
public interface IPeerConnector : IDisposable { }


---


This int type get set property represents The maximum amount of peers which the node can connect to (defaults to 8).

int MaximumNodeConnections { get; set; }

---


This get property represents The collection of peers which the node is currently connected to.

NetworkPeerCollection ConnectedPeers { get; }

---


This get set property represents Other peer connectors which this instance relates. 
        
This is used to ensure that the same IP doesn't get connected to in this connector.
        
        
RelatedPeerConnectors RelatedPeerConnector { get; set; }


---


This get property represents Specification of requirements which the "PeerConnector" should have when connecting to other peers.

NetworkPeerRequirement Requirements { get; }


---


This method  Adds a peer to the "ConnectedPeers.
        
This will only happen if the peer successfully handshaked with another.
        
        
void AddPeer(NetworkPeer peer);


---


This method Removes a given peer from the "ConnectedPeers.

This will happen if the peer state changed to "disconnecting", "failed" or "offline".
        
        
void RemovePeer(NetworkPeer peer);


---

This method Starts an asynchronous loop that connects to peers in one second intervals.

If the maximum amount of connections has been reached ("MaximumNodeConnections), the action gets skipped.
        
void StartConnectAsync();
		
		
---		


This class is for Connects to peers asynchronously, filtered by "PeerIntroductionType.
    
public sealed class PeerConnector : IPeerConnector { }


---


This field represents The async loop which we need to wait upon before we can dispose of this connector.

private IAsyncLoop asyncLoop;


---


This field is for Factory for creating background async loop tasks.

private readonly IAsyncLoopFactory asyncLoopFactory;


---


This field represents The cloned parameters used to connect to peers. 

private readonly NetworkPeerConnectionParameters currentParameters;

---


This field represents How to calculate a group of an IP, by default using NBitcoin.IpExtensions.GetGroup.

private readonly Func<IPEndPoint, byte[]> groupSelector;

---

This field represents Global application life cycle control - triggers when application shuts down.

private readonly INodeLifetime nodeLifetime;

---

This field represents The network which the node is running on.

private Network network;

---

This field represents The network peer parameters that is injected by "Connection.ConnectionManager.

private readonly NetworkPeerConnectionParameters parentParameters;

---

This field represents Peer address manager instance, see "IPeerAddressManager".

private readonly IPeerAddressManager peerAddressManager;
		
		
---

This field represents What peer types (by "PeerIntroductionType" this connector should find and connect to.)

private readonly PeerIntroductionType peerIntroductionType;

---

This field represents Factory for creating P2P network peers.

private readonly INetworkPeerFactory networkPeerFactory;

---

This constructor is Constructor whic is used for unit testing.

internal PeerConnector(IPeerAddressManager peerAddressManager, PeerIntroductionType peerIntroductionType) { }

---


This constructor is Constructor used by dependency injection.

IPeerAddressManager peerAddressManager,
internal PeerConnector(
Network network, 
INodeLifetime nodeLifeTime, 
NetworkPeerConnectionParameters parameters, 
NetworkPeerRequirement requirements, 
Func<IPEndPoint, byte[]> groupSelector, 
IAsyncLoopFactory asyncLoopFactory, 
PeerIntroductionType peerIntroductionType, 
INetworkPeerFactory networkPeerFactory) { }


---

This method Attempts to connect to a random peer.

private Task ConnectAsync() { }

---

This method Disconnects all the peers in "ConnectedPeers".

private void Disconnect() { }


---


This method Selects a peer from the address manager.
        
Refer to "IPeerAddressManager.SelectPeerToConnectTo(PeerIntroductionType)" for details on how this is done.
        
internal NetworkAddress FindPeerToConnectTo() { } 


---

