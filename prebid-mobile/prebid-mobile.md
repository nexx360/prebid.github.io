---
layout: page_v2
title: Prebid Mobile
description: What is Prebid.js
sidebarType: 2
---

# Prebid Mobile Overview
{:.no_toc}

Prebid Mobile is an open-source library for iOS and Android that provides an end-to-end header bidding solution for mobile app publishers.

---- 

{: .alert.alert-info :}
The Prebid Mobile team is pleased to inform you that `Prebid Mobile 3.0` is live.

Here is what you need to know first about the new version:

- [3.0 Updates Overview](updates-3.0/sdk-key-features.html).
- API Changes: [iOS](updates-3.0/ios/api-changes.html), [Android](updates-3.0/android/api-changes.html).

Provide feedback to <support@prebid.org>.

---- 

- TOC
{:toc}

## Video Overview of Prebid Mobile

A high-level overview of Prebid Mobile, Prebid’s header bidding product for iOS and Android applications.

{% include vimeo-iframe.html id="822158733" title="1.4_Intro-to-Prebid-Mobile_v3" %}

Further Content:

- [Transcript of this video](/prebid-mobile/prebid-mobile-video.html)
- [Prebid Managed Services](https://prebid.org/product-suite/managed-services/)

## Benefits and Features

Prebid SDK rendering offers the following benefits:

- **Transparent, open source header bidding solution**. The single integration point with Prebid Server, enabling direct access to more mobile buyers.
- **Monetization without an Ad Server**: Publishers who do not have a direct sales force or have no need for an ad server can still access Prebid's mobile demand stack. Publishers will be able to render ads directly without relying on any 3rd party SDKs.
- **Reduced ad delivery latency**: The rendering module enables Prebid SDK to render ads immediately when demand is returned from Prebid Server or when receiving the render signal from an ad server. The render process should vastly reduce ad delivery speeds.
- **Less infrastructure**: The rendering API does not rely on Prebid Server's Cache server, reducing the cost and utility of Prebid Server Cache.
- **Framework support**: Full support of SKAdNetworks and similar frameworks
- **MRAID 3.0 support**
- **Flexible Ad Measurement**: Controlling the rendering and Open Measurement process allows publishers to potentially configure any measurement provider in a transparent and open source process. Prebid SDK will eventually be IAB Open Measurement certified.
- **Ad Server Support**: can be integrated in any potential monetization stack.
- **Community driven**: Being a part of Prebid, there is the ability to add features not readily or easily available either through the Ad Server or other SDKs

## How It Works

Prebid SDK supports following integration scenarios:

- **Custom or No mediation** when no Primay Ad Server is used. The SDK renders the winning bid immediately when it is available.
- **Using a Primary Ad Server** - There are three aproaches are avaliable for integration with primary ad server
  - **Bidding-Only Integration (GAM)** - Prebid SDK plays a transport role of the bid from PBS to Google Ad Manager where it takes place in the internal auction and the winning bid is rendered later with Prebid Universal Creative. (This method was formerly known as the "Original API")
  - **Prebid-Rendered Integration** - Prebid SDK is called when a Prebid line item wins on the ad server and renders the bid in a WebView or VideoView. (This method was formerly known as the "Rendering API")
  - **Mediation API** - Prebid SDK is called by the mediation platform, rendering any winning bid.

In all scenarios, Prebid SDK leverages Prebid Server for demand. Additional details about each scenario are available on the platform-specific pages.

The following chart shows which approaches may be used with various ad servers.

{: .table .table-bordered .table-striped }

|Ad Server|Bidding-Only Method|Prebid-Rendered Method|Mediation API|
|------------|:------------:|:-------------:|:-------------:|
|No Ad Server|            |      ✅   |             |
|GAM         |     ✅   |      ✅   |             |
|AdMob       |            |             |     ✅   |
|MAX         |            |             |     ✅   |

The following sections describe each integration method.

### No Ad Server

![In-App Rendering](/assets/images/prebid-mobile/modules/rendering/Prebid-In-App-Bidding-Overview-Pure-Prebid.png){: .pb-lg-img :}

1. The Prebid SDK sends the bid request to the Prebid Server
1. Prebid Server runs the header bidding auction among preconfigured demand partners
1. Prebid Server responds with the winning bid
1. The rendering module renders the winning bid

### GAM Bidding-Only Integration

![How Prebid Mobile Works - Diagram](/assets/images/prebid-mobile/prebid-in-app-bidding-overview-prebid-original.png)
{: .pb-lg-img :}

1. Prebid SDK sends a request to Prebid Server. This request consists of the Prebid Server account ID and config ID.
1. Prebid Server constructs an OpenRTB bid request and passes it to the demand partners. Each demand partner returns a bid response to Prebid Server. The bid response includes the bid price and the creative content.
1. Prebid Server sends the bid responses to Prebid SDK.
1. Prebid SDK sets key/value targeting for each ad slot through the GMA SDK.
1. The GMA SDK sends the ad request enriched with targeting keywords of the wining bid.
1. GAM responds with an ad. If the line item associated with the Prebid Mobile bid wins, the primary ad server returns the Prebid Universal Creative (PUC) to the ad server's SDK.
1. GMA SDK starts the rendering recived ad markup.
1. The PUC fetches creative content of the winning bid from the Previd Cache and renders it.

### GAM Prebid-Rendered Integration

![Rendering with Primary Ad Server](/assets/images/prebid-mobile/modules/rendering/In-App-Bidding-Integration.png)
{: .pb-lg-img :}

1. The rendering module sends the bid request to the Prebid Server.
1. Prebid Server runs the header bidding auction among configured demand partners.
1. Prebid Server responds with the winning bid that contains targeting keywords.
1. The Prebid SDK rendering module sets up the targeting keywords of the winning bid into the ad unit of the GMA SDK.
1. The GMA SDK sends the ad request to GAM.
1. GAM responds with an ad.
1. If the winning ad is from Prebid, it's passed to the Prebid SDK rendering module.
1. Depending on the ad response, the Prebid SDK rendering module renders the winning bid or allows the GMA SDK to show its own winning ad.

### Mediation Platforms

Supported Platformw: AdMob, MAX.

![How Prebid Mobile Works - Diagram](/assets/images/prebid-mobile/prebid-in-app-bidding-overview-mediation.png)
{: .pb-lg-img :}

1. The Prebid SDK sends the bid request to the Prebid Server.
1. Prebid Server runs the header bidding auction among preconfigured demand partners.
1. Prebid Server responds with the winning bid that contains targeting keywords.
1. The mediation SDK sends the ad request to the mediation platform, which responds with a mediation chain.
1. The mediation SDK runs through the mediation chain.
1. If the mediation item is for Prebid, it instantiates the respective class.
1. The appropriate Prebid SDK 'adapter' renders a wining bid cached in the SDK.

## Integration Reference

### Prebid Server

You must have a Prebid Server available in order to use Prebid Mobile. Prebid Server is a server-based host that communicates bid requests and responses between Prebid Mobile and demand partners.  

To set up your Prebid Server account for Prebid Mobile, refer to [Getting Started with Prebid Mobile](/prebid-mobile/prebid-mobile-getting-started.html).

### Android

Follow these steps to integrate the Prebid SDK:

1. [Integrate Prebid SDK](pbm-api/android/code-integration-android.html) into your project.
1. Define [global integration and targeting properties](/prebid-mobile/pbm-api/android/pbm-targeting-android.html).  
1. Add Prebid's ad units to your app respectively to the monetization scenario:
    - [GAM Bidding-Only Integration](pbm-api/android/android-sdk-integration-gam-original-api.html)
    - [Custom in-app bidding](modules/rendering/android-sdk-integration-pb.html) integration without primary ad server.
    - [GAM Prebid-Rendered Integration](modules/rendering/android-sdk-integration-gam.html) as a primary ad server
    - [AdMob](modules/rendering/android-sdk-integration-admob) as a primary ad server.
    - [AppLovin MAX](modules/rendering/android-sdk-integration-max.html) as a primary ad server.
1. Create the ad server or mediation platform setup:
    - [GAM Bidding-Only Integration](../adops/step-by-step.html)
    - [GAM Prebid-Rendered Integration](../adops/mobile-rendering-gam-line-item-setup.html)
    - [AdMob](../adops/mobile-rendering-admob-line-item-setup.html)
    - [MAX](../adops/mobile-rendering-max-line-item-setup.html)

### iOS

Follow these steps to integrate the rendering API:

1. [Integrate Prebid SDK](pbm-api/ios/code-integration-ios.html).
1. Define [global integration and targeting properties](/prebid-mobile/pbm-api/ios/pbm-targeting-ios.html).
1. Add prebid's ad units to your app respectively to the monetization scenario:
    - [GAM Bidding-Only Integration](pbm-api/ios/code-integration-ios.html)
    - [Custom in-app bidding](modules/rendering/ios-sdk-integration-pb.html) integration without a primary ad server.
    - [GAM Prebid-Rendered Integration](modules/rendering/ios-sdk-integration-gam.html) as a primary ad server.
    - [AdMob](modules/rendering/ios-sdk-integration-gam.html) as a primary ad server.
    - [AppLovin MAX](modules/rendering/ios-sdk-integration-max.html) as a primary ad server.
1. Create the ad server or mediation platform setup:
    - [GAM Bidding-Only Integration](../adops/step-by-step.html)
    - [GAM Prebid-Rendered Integration](../adops/mobile-rendering-gam-line-item-setup.html)
    - [AdMob](../adops/mobile-rendering-admob-line-item-setup.html)
    - [MAX](../adops/mobile-rendering-max-line-item-setup.html)

## Mobile Analytics

Currently Prebid Mobile SDK doesn't offer direct analytics capabilities. While we build out analytics in Prebid Server to support the SDK, some options are:

- Generate analytics from the ad server, as key metrics are available there if the line items are broken out by bidder.
- Integrate an analytics package directly into the app. You may have one already that can accommodate header bidding metrics.
- Utilize a server-side [analytics module for Prebid Server](/prebid-server/developers/pbs-build-an-analytics-adapter.html).

## Further Reading

- [Getting started with Prebid Mobile](/prebid-mobile/prebid-mobile-getting-started.html)
- [Deep Links Support](modules/rendering/rendering-deeplinkplus.html)
- [Impression Tracking](modules/rendering/rendering-impression-tracking.html)
