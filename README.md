# SpockEx

## Introduction

SpockEx is short for "Spock Exchange", which is itself a play on "Stock Exchange". The idea is a mashup of two concepts: one of trading and commerce, and the other from the post-capitalist economy in the fictional Star Trek universe. Thus, the "Spock Exchange" is intended as a means of exchange of goods and services that bypasses rather than replaces the need for money.

The SpockEx application is intended to be a tool for connecting peers on a distributed, non-hierarchical database, for the purposes of sharing and storing information redundantly. The intended data to be shared can be, but is not limited to, records of freely-shared goods or services (or assets), along with nested records of the freely-shared assets that serve as components for the resulting asset, the data for which can be stored locally or by the peers who provided each component asset.

## Protocols

### [Entanglement Protocol Specification (alpha)](docs/Entanglement.md)

The process for connecting peers.

### Protocol for Defining Basic Asset Components. (Coming Soon)

If you make a cake from scratch, components will be eggs, flour, sugar, human labor, etc. We need a why to poll peers to establish values, demand, etc.

### Composite Asset Creation Protocol (Coming Soon)

How new assets are defined, either from predefined primative components, or from other composite components. 

### Asset Offer Protocol (Coming Soon)

Once one has an asset, one my offer any surplus to the community.

### Asset Request Protocol (Coming Soon)

Collect and display any offers to the community.

### Asset Acceptance Protocol (Coming Soon)

How assets are accepted and marked as complete.

### Publish/Syncronize Protocol (Coming Soon)

Will likely use ZeroMQ for publishing to established peers.

### Share Ratio Protocol (Coming Soon)

With data now syncronized, we must assign share ratios (giving vs recieving) to peers to inform the community's future sharing decisions.
