# EIP-0024: Artwork Contract

> 🔗 From [EIP-0024:](https://github.com/ergoplatform/eips/blob/master/eip-0024.md)


## Motivation 
With the discovery that we can easily use spent boxes in contracts ([see here](https://www.ergoforum.org/t/ergoscript-design-patterns/222/23?u=anon_real)), some new features are introduced for artworks and can be extended furthur in the future. This EIP is an extension of [Asset standard](eip4.md) and tents to standardize artwrok issuance in particular.


## Design
There are two important boxes in the issuance process of an artwork.
- The issuance box, which follows [Asset standard](eip4.md) depending on the type of artwork.
- The issuer box, which is the first input of the transaction that is issuing the artwork. In particular, the box with the same ID as the artwork's token ID.

Now we will discuss why the issuer box is important and define a standard for it.

The issuer box is important because it has the same ID as the artwork's token ID and we can include it (although it is spent) in our transactions either in registers or as context data. This means that we can prove/verify certain features that an artwork may have using this box. The following is the current standard for this box - which may be extended in the future.

- R4 of this box is an Int, showing 1000 * royalty percentage of the artwork. e.g, 20 for 2% royalty. If R4 of the issuer box of an artowrk is empty or non-positive, then royalty is considered to be 0%.
- Proposition bytes of this box is the contract that the royalty percentage will be sent to. In case of a simple proxy contract this means that the artist will recieve the royalty share. However, the proxy contract may inforce the royalty to go to any other complex contract - e.g., 20% to the artrist, 80% to some charity.

## Artist Identity
Artist identity is very important for any auction hosue to eliminate NFT copies and scams for the end users.

The identity of the artist can not necessarily be determined with the issuance box (unless it is a P2PK box) since its correctness can not be verified, e.g., one can impersonate an artist.

Hence, the artist identity and address is determined with the first P2PK input in the chain of transactions leading to the artwork issuance. This means that in case of using proxy contracts, the first input of the transaction that is sending the funds to the proxy contract is going to determine the artist's identity.