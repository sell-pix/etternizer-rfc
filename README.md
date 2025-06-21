# ⚛️ Image Upload & Watermark Processing Pipeline RFC

This RFC proposes the architecture for a reliable, scalable image upload and watermarking pipeline designed for a SaaS platform that enables photographers to sell protected previews of their work. The system supports high-resolution image uploads, automatic watermarking, metadata storage, and delivery through a CDN.

## Summary

- [Problem description](#Problem-description)
- [Personas](#Personas)
- [Usability](#Usability)
- [Architecture Overview](#Architecture-Overview)
- [Profitability](#Profitability)

## Problem description

Photographers need a way to sell digital images online without exposing full-resolution files before purchase. The current manual workflow for uploading, watermarking, and delivering images is time-consuming, error-prone, and does not scale well.

This system should:

- Automatically apply watermarks to previews.

- Deliver original files securely after payment.

- Organize images by album/theme.

- Minimize manual effort and storage costs.


## Personas

- *Photographer*
- *Customer*
- *System Admin*

<br/>

- The *Photographer* wants to upload high-res photos, organize them, and have previews protected. Prefers automation and wants a professional storefront experience.
- The *Customer* wants to browse and preview photos before buying, pay quickly (via Pix), and get instant access to purchased files.
- The *System Admin* will maintain and monitor the system for performance, cost, and abuse.

## Usability

<img src="./usability.png">

Uploads should support batch/folder drag-and-drop.

Customers should receive watermarked previews after processing.

Purchased photos should be emailed securely with temporary download links.

The system should scale without degrading UX.


### Architecture Overview

| Name | Description |
| - | - |
| Amount exceeds the QRCode limit | happens when the `currentValueAmount` is less than the value that the **receiver** tried to receive in the transaction |
| Invalid QRCode | happens when the QRCode was expired or manually invalidated by the **emitter** through the **intermediator** application/webservice |
| Security Exception | something wrong was found during the transaction and it was forcibly cancelled to avoid bigger problems, in that case the QRCode may be invalidated automatically |

## Security

There are some security cares that may be taken during the implementation of the QRCode and the system itself, starting by location-based checks before every transaction. Security exceptions may be raised when: a first transaction was made at least 500 kilometers from another between a very short period - usually 1 hour or a first transaction is trying to be executed from a place too far from where the QRCode was created (usually 500 kilometers) in a very short period, also usually 1 hour. The **intermediator** should also be prepared to take care about brute force attacks, so it may be prepared to identify different kinds of payloads trying to execute in a very short periods of time with a field in common. Besides that, the **intermediator** should also be able to invalidate any created QRCode from outside the main application/webservice for any reason and also block future QRCode creations if it is explicitly desired by the client.

## Profitability

There are some points from where the **intermediator** can profit providing the offline pix feature:

- *transfer fees* - fees that are applied to every transference during the utilization of the QRCode, it's a small fee, usually between 0.3% to 0.5% of the amound applied as additional value;
- *creational fees* - a fee that is applied during the QRCode creation, it can be a small percentage (1% to 1.5%) of the total amound value grabbed in the QRCode
- *market partnership* - being partner with selling companies to offer distinguished treatment and easily implement the offline pix, charging a small amount per month
- *charging the sellers* - making the feature paid for the stores and the sellers that want to accept offline pix payments and keeping it free for the users