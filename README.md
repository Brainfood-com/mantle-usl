## Mantle Business Artifacts - Universal Service Library


[![license](http://img.shields.io/badge/license-CC0%201.0%20Universal-blue.svg)](https://github.com/moqui/mantle-usl/blob/master/LICENSE.md)
[![build](https://travis-ci.org/moqui/mantle-usl.svg)](https://travis-ci.org/moqui/mantle-usl)
[![release](http://img.shields.io/github/release/moqui/mantle-usl.svg)](https://github.com/moqui/mantle-usl/releases)
[![commits since release](http://img.shields.io/github/commits-since/moqui/mantle-usl/v2.2.0.svg)](https://github.com/moqui/mantle-usl/commits/master)

[![Discourse Forum](https://img.shields.io/badge/moqui%20forum-discourse-blue.svg)](https://forum.moqui.org)
[![Google Group](https://img.shields.io/badge/google%20group-moqui-blue.svg)](https://groups.google.com/d/forum/moqui)
[![LinkedIn Group](https://img.shields.io/badge/linked%20in%20group-moqui-blue.svg)](https://www.linkedin.com/groups/4640689)
[![Stack Overflow](https://img.shields.io/badge/stack%20overflow-moqui-blue.svg)](http://stackoverflow.com/questions/tagged/moqui)

For details about Mantle see its page on moqui.org:

<http://www.moqui.org/mantle.html>

There is an overview of Mantle Business Artifacts in the **Making Apps with Moqui** book. Download the PDF here:

<http://www.moqui.org/MakingAppsWithMoqui-1.0.pdf>

---

### Dropshipping Setup
Difference from the standard setup:
- Set the `Facility`'s drop-ship `facilityTypeEnumId` to `FcTpDropShip`
- Set the `Facility`'s `assetAllowOtherOwner` to `'Y'`
- Set the `Facility`'s `ownerPartyId` to the `Party` that will be the order vendor party
- For each `OrderItem` to drop-ship, set the `OrderPart` `Facility` or if not the `Facility`, the `ProductStoreFacility` the facility to drop-ship `Facility` selected before (if using the normal asset reservation service `AssetServices.reserve#AssetForOrderItem`)
- Reserve the assets for your `OrderItem`s as usual to the `Facility` selected before by Placing / Approving / manually reserving the inventory
- 
