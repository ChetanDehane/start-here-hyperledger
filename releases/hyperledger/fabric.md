---
layout: default
title: fabric
parent: Hyperledger
grand_parent: Releases
has_children: false
permalink: /releases/hyperledger/fabric
---

# fabric <span class="fs-3 right-align">[GitHub](https://github.com/hyperledger/fabric){: .btn .mr-4 }</span>


<div>
    <table>
        <tr>
            <td colspan="2">
                <b>
                    v2.4.3
                </b>
            </td>
        </tr>
        <tr>
            <td>
                <span class="chip">
                    v2.4.3
                </span>
            </td>
            <td>
                v2.4.3 Release Notes - February 25, 2022
========================================


Fixes
-----

**peer - Add Intermediate CA certificates to gateway dial options**

The gateway service was not including TLS intermediate certificates in the dial options when connecting to other nodes,
resulting in TLS handshake errors between the gateway peer and other nodes.
The gateway service now includes TLS root certificates and intermediate certificates when connecting to other nodes.

**peer - Gateway support for mutual TLS**

The gateway service was not passing the peer's client certificate when connecting to other nodes.
To support TLS client authentication (mutual authentication), the gateway service now passes the peer's
client certificate (if one is configured) when connecting to other nodes.

**peer and orderer - Handle TLS CA certificate expiry**

When any TLS CA certificate configured on a channel expired,
peer and orderer nodes fail to start due to MSP initialization error
"setting up the MSP manager failed: CA Certificate is not valid: certificate has expired or is not yet valid".
MSP initialization now ignores TLS CA certificate expiration so that the peer
or orderer can start up and receive channel configuration updates with renewed TLS CA certificates.


Dependencies
------------
Fabric v2.4.3 has been tested with the following dependencies:
* Go 1.17.5
* CouchDB v3.1.1

Fabric docker images on dockerhub utilize Alpine 3.14.


Deprecations (existing)
-----------------------

**Ordering service system channel is deprecated**

v2.3 introduced the ability to manage an ordering service without a system channel.
Managing an ordering service without a system channel has privacy, scalability,
and operational benefits. The use of a system channel is deprecated and may be removed in a future release.
For information about removal of the system channel, see the [Create a channel without system channel documentation](https://hyperledger-fabric.readthedocs.io/en/release-2.3/create_channel/create_channel_participation.html).

**FAB-15754: The 'Solo' consensus type is deprecated.**

The 'Solo' consensus type has always been marked non-production and should be in
use only in test environments, however for compatibility it is still available,
but may be removed entirely in a future release.

**FAB-16408: The 'Kafka' consensus type is deprecated.**

The 'Raft' consensus type was introduced in v1.4.1 and has become the preferred
production consensus type.  There is a documented and tested migration path from
Kafka to Raft, and existing users should migrate to the newer Raft consensus type.
For compatibility with existing deployments, Kafka is still supported,
but may be removed entirely in a future release.
Additionally, the fabric-kafka and fabric-zookeeper docker images are no longer updated, maintained, or published.

**Fabric CouchDB image is deprecated**

v2.2.0 added support for CouchDB 3.1.0 as the recommended and tested version of CouchDB.
If prior versions are utilized, a Warning will appear in peer log.
Note that CouchDB 3.1.0 requires that an admin username and password be set,
while this was optional in CouchDB v2.x. See the
[Fabric CouchDB documentation](https://hyperledger-fabric.readthedocs.io/en/v2.2.0/couchdb_as_state_database.html#couchdb-configuration)
for configuration details.
Also note that CouchDB 3.1.0 default max_document_size is reduced to 8MB. Set a higher value if needed in your environment.
Finally, the fabric-couchdb docker image will not be updated to v3.1.0 and will no longer be updated, maintained, or published.
Users can utilize the official CouchDB docker image maintained by the Apache CouchDB project instead.

**FAB-7559: Support for specifying orderer endpoints at the global level in channel configuration is deprecated.**

Utilize the new 'OrdererEndpoints' stanza within the channel configuration of an organization instead.
Configuring orderer endpoints at the organization level accommodates
scenarios where orderers are run by different organizations. Using
this configuration ensures that only the TLS CA certificates of that organization
are used for orderer communications, in contrast to the global channel level endpoints which
would cause an aggregation of all orderer TLS CA certificates across
all orderer organizations to be used for orderer communications.

**FAB-17428: Support for configtxgen flag `--outputAnchorPeersUpdate` is deprecated.**

The `--outputAnchorPeersUpdate` mechanism for updating anchor peers has always had
limitations (for instance, it only works the first time anchor peers are updated).
Instead, anchor peer updates should be performed through the normal config update flow.

**FAB-15406: The fabric-tools docker image is deprecated**

The fabric-tools docker image will not be published in future Fabric releases.
Instead of using the fabric-tools docker image, users should utilize the
published Fabric binaries. The Fabric binaries can be used to make client calls
to Fabric runtime components, regardless of where the Fabric components are running.

**FAB-15317: Block dissemination via gossip is deprecated**

Block dissemination via gossip is deprecated and may be removed in a future release.
Fabric peers can be configured to receive blocks directly from an ordering service
node and not gossip blocks by using the following configuration:
```
peer.gossip.orgLeader: true
peer.gossip.useLeaderElection: false
peer.gossip.state.enabled: false
peer.deliveryclient.blockGossipEnabled: false
```

**FAB-15061: Legacy chaincode lifecycle is deprecated**

The legacy chaincode lifecycle from v1.x is deprecated and will be removed
in a future release. To prepare for the eventual removal, utilize the v2.x
chaincode lifecycle instead, by enabling V2_0 application capability on all
channels, and redeploying all chaincodes using the v2.x lifecycle. The new
chaincode lifecycle provides a more flexible and robust governance model
for chaincodes. For more details see the
[documentation for enabling the new lifecycle](https://hyperledger-fabric.readthedocs.io/en/release-2.2/enable_cc_lifecycle.html).


## Changes:

* 9711fb5d0c16297584f5c53123f589110828736f Release commit for v2.4.3
* 47dd17a735118c306c59310fc21f3648e3676e3b Ignore expired CA/TLS CA certs on msp init (#3238) (#3249) (#3252)
* c89ba6048c06ec9ace260733d862112da84f35bb Gateway support for mutual TLS networks (#3235)
* 602f4c6df942faaddb95e153a8c582ed9858b8c5 remove commercial paper references
* 80dbf8ebaff1694ab0ca6c7ebe6bca4513be17a2 Add Intermediate CA certs to dial options (#3225) (#3226)

This list of changes was [auto generated](https://dev.azure.com/Hyperledger/Fabric/_build/results?buildId=48768&view=logs).
            </td>
        </tr>
    </table>
    <a href="https://github.com/hyperledger/fabric/releases/tag/v2.4.3" class=".btn">
        View on GitHub
    </a>
    <span class="right-align">
        Created At 2022-02-25 13:40:46 +0000 UTC
    </span>
</div>

