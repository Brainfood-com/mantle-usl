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
Requires commit after https://github.com/moqui/mantle-usl/commit/943e8f90d59206056114f0574ac9075f5b280c07
Difference from the standard setup:
- Set the `Facility`'s drop-ship `facilityTypeEnumId` to `FcTpDropShip`
- Set the `Facility`'s `assetAllowOtherOwner` to `'Y'`
- Set the `Facility`'s `ownerPartyId` to the `Party` that will be the order vendor party
- For each `OrderItem` to drop-ship, set the `OrderPart` `Facility` or if not the `Facility`, the `ProductStoreFacility` the facility to drop-ship `Facility` selected before (if using the normal asset reservation service `AssetServices.reserve#AssetForOrderItem`)
- Reserve the assets for your `OrderItem`s as usual to the `Facility` selected before by Placing / Approving / manually reserving the inventory
- Create a `SystemMessageRemote` for the drop-shipper facility, and connect it to the `Facility` through the `dropShipRemoteId` parameter
- Create a service that will create the purchase order for the drop-shipper
```xml
<service verb="send" noun="OrderToExampleName">
    <implements service="mantle.order.OrderServices.send#DropShipPurchaseOrderPart"/>
    <actions>
        <!-- ... Send your order to the 3rd party dropshipper ... -->
        <entity-find-one entity-name="mantle.facility.Facility" value-field="facilityId" auto-field-map="[facilityId:facilityId]"/>
        <if condition="!facility"><return error="true" message="No facility found for ${facilityId}"/></if>
        <if condition="!facility.dropShipRemoteId"><return error="true" message="No dropShipRemoteId found for ${facilityId}"/></if>
        <entity-find-one entity-name="moqui.service.message.SystemMessageRemote" value-field="remote" auto-field-map="[systemMessageRemoteId:facility.dropShipRemoteId]"/>
        <if condition="!remote"><return error="true" message="No remote found for ${facility.dropShipRemoteId}"/></if>

        <!-- ... Use the configuration here to connect to your drop-shipper api ... -->
    </actions>
</service>
```
- Create service that will handle the shipment creation when the dropshipper calls your API (or a service-job runs on a time interval)
For example:
  - Receive rest call: https://github.com/moqui/mantle-shippo/blob/3c646e4492c423a47edf4c9eb4cf5995e30c9a9c/service/shippo.rest.xml#L24
  - Create the shipment: https://github.com/moqui/mantle-shippo/blob/3c646e4492c423a47edf4c9eb4cf5995e30c9a9c/service/mantle/shippo/ShippoServices.xml#L1699
The key here is to get the tracking number or some sort of external id from the drop-shipper so you and your customer can know when the package will arrive and if it does to keep the drop-shipper accountable for the delivery (if you need that).
