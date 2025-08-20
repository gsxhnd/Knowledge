---
title: "WebRTC API"
source: "https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API"
author:
  - "[[MozDevNet]]"
published: 2025-02-07
created: 2025-08-20
description: "WebRTC（Web 实时通信）是一种使 Web 应用程序和站点能够捕获和选择性地流式传输音频或视频媒体，以及在浏览器之间交换任意数据的而无需中间件的技术。WebRTC 的一系列标准使得在不需要用户安装插件或任何其他第三方软件的情况下，可以实现点对点数据共享和电话会议。"
tags:
  - "clippings"
---
WebRTC 有多种用途；与 [媒体捕捉与媒体流 API](https://developer.mozilla.org/zh-CN/docs/Web/API/Media_Capture_and_Streams_API) 一起使用时，它们为 Web 提供了强大的多媒体功能，包括支持音频和视频会议、文件交换、屏幕共享、身份管理以及与传统电话系统的接口，包括发送 [DTMF](https://developer.mozilla.org/zh-CN/docs/Glossary/DTMF) （按键拨号）信号。两个对等方之间的连接可以在不需要任何特殊驱动程序或插件的情况下建立，并且通常可以在没有任何中间服务器的情况下建立连接。

两个对等方之间的连接由 [`RTCPeerConnection`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection) 接口表示。一旦使用 `RTCPeerConnection` 建立并打开了连接，就可以向连接添加媒体流（ [`MediaStream`](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaStream) ）或数据通道（ [`RTCDataChannel`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCDataChannel) ）。

媒体流可以由任意数量的媒体信息轨道组成；轨道由基于 [`MediaStreamTrack`](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaStreamTrack) 接口的对象表示，可以包含多种类型的媒体数据，包括音频、视频和文本（例如字幕甚至章节名称）。大多数流至少包含一个音频轨道和可能也包含一个视频轨道，并且可以用于发送和接收实时媒体或存储的媒体信息（例如流媒体电影）。

你还可以使用两个对等方之间的连接来交换任意二进制数据，使用 [`RTCDataChannel`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCDataChannel) 接口。这可以用于反向信道信息、元数据交换、游戏状态数据包、文件传输，甚至作为数据传输的主要通道。

WebRTC 在现代浏览器中通常得到很好的支持，但仍存在一些不兼容性。 [adapter.js](https://github.com/webrtcHacks/adapter) 库是一个可以使应用程序免受这些不兼容性的影响的垫片。

由于 WebRTC 提供了一系列共同工作以完成各种任务的接口，我们将参考资料按类别进行了划分。请参阅侧边栏中的按字母顺序排列的列表。

这些接口、字典和类型用于设置、打开和管理 WebRTC 连接。包括表示对等媒体连接、数据通道和在交换每个对等方的能力信息以选择最佳配置的接口时使用的接口。

#### 接口

[`RTCPeerConnection`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection)

表示本地计算机与远程对等方之间的 WebRTC 连接。它用于处理两个对等方之间的数据流式传输。

[`RTCDataChannel`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCDataChannel)

表示连接的两个对等方之间的双向数据通道。

[`RTCDataChannelEvent`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDataChannelEvent)

表示在将 [`RTCDataChannel`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCDataChannel) 附加到 [`RTCPeerConnection`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection) 时发生的事件。使用此接口发送的唯一事件是 [`datachannel`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection/datachannel_event "datachannel") 。

[`RTCSessionDescription`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCSessionDescription)

表示会话的参数。每个 `RTCSessionDescription` 包括一个描述 [`type`](https://developer.mozilla.org/en-US/docs/Web/API/RTCSessionDescription/type "type") ，指示其描述的提议/应答协商过程的哪一部分，以及会话的 [SDP](https://developer.mozilla.org/zh-CN/docs/Glossary/SDP) 描述符。

[`RTCStatsReport`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCStatsReport)

提供有关连接或连接上的个别轨道的统计信息的详细信息；可以通过调用 [`RTCPeerConnection.getStats()`](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection/getStats) 来获取报告。

[`RTCIceCandidate`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCIceCandidate)

表示用于建立 [`RTCPeerConnection`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection) 的候选交互式连接建立（ [ICE](https://developer.mozilla.org/zh-CN/docs/Glossary/ICE) ）服务器。

[`RTCIceTransport`](https://developer.mozilla.org/en-US/docs/Web/API/RTCIceTransport)

表示有关 [ICE](https://developer.mozilla.org/zh-CN/docs/Glossary/ICE) 传输的信息。

[`RTCPeerConnectionIceEvent`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnectionIceEvent)

表示与目标 ICE 候选项相关的事件，通常是 [`RTCPeerConnection`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection) 。此类型仅有一个事件： [`icecandidate`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection/icecandidate_event "icecandidate") 。

[`RTCRtpSender`](https://developer.mozilla.org/en-US/docs/Web/API/RTCRtpSender)

管理在 [`RTCPeerConnection`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection) 上的 [`MediaStreamTrack`](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaStreamTrack) 的数据编码和传输。

[`RTCRtpReceiver`](https://developer.mozilla.org/en-US/docs/Web/API/RTCRtpReceiver)

管理在 [`RTCPeerConnection`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection) 上的 [`MediaStreamTrack`](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaStreamTrack) 的数据接收和解码。

[`RTCTrackEvent`](https://developer.mozilla.org/en-US/docs/Web/API/RTCTrackEvent)

用于表示 [`track`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection/track_event "track") 事件的接口，该事件指示已将 [`RTCRtpReceiver`](https://developer.mozilla.org/en-US/docs/Web/API/RTCRtpReceiver) 对象添加到 [`RTCPeerConnection`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection) 对象，表示已创建并添加了新的传入 [`MediaStreamTrack`](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaStreamTrack) 至 `RTCPeerConnection` 。

[`RTCSctpTransport`](https://developer.mozilla.org/en-US/docs/Web/API/RTCSctpTransport)

提供描述流控制传输协议（ **[SCTP](https://developer.mozilla.org/zh-CN/docs/Glossary/SCTP)** ）传输的信息，并提供一种访问底层的用于所有 [`RTCPeerConnection`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection) 的数据通道发送和接收的 SCTP 包的数据报传输层安全（ **[DTLS](https://developer.mozilla.org/zh-CN/docs/Glossary/DTLS)** ）传输的方式。

#### 事件

[`bufferedamountlow`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDataChannel/bufferedamountlow_event "bufferedamountlow")

表示数据通道当前缓冲的数据量（由其 [`bufferedAmount`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDataChannel/bufferedAmount "bufferedAmount") 属性指示）已经减少到或低于通道的最小缓冲数据大小（由 [`bufferedAmountLowThreshold`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDataChannel/bufferedAmountLowThreshold "bufferedAmountLowThreshold") 指定）。

[`close`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDataChannel/close_event "close")

数据通道已完成关闭过程，现在处于 `closed` 状态。此时，其底层数据传输完全关闭。你可以通过观察 `closing` 事件来在关闭完成 *之前* 得到通知。

[`closing`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDataChannel/closing_event "closing")

`RTCDataChannel` 已转换为 `closing` 状态，表示它将很快关闭。你可以通过观察 `close` 事件来检测关闭过程的完成。

[`connectionstatechange`](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection/connectionstatechange_event "connectionstatechange")

连接状态（可以通过 [`connectionState`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection/connectionState "connectionState") 访问）已更改。

[`datachannel`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection/datachannel_event "datachannel")

有一个新的 [`RTCDataChannel`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCDataChannel) 在远程对等方打开新数据通道之后可用。此事件的类型是 [`RTCDataChannelEvent`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDataChannelEvent) 。

[`error`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDataChannel/error_event "error")

表示数据通道上发生错误的 [`RTCErrorEvent`](https://developer.mozilla.org/en-US/docs/Web/API/RTCErrorEvent) 。

[`error`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDtlsTransport/error_event "error")

表示 [`RTCDtlsTransport`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDtlsTransport) 上发生错误的 [`RTCErrorEvent`](https://developer.mozilla.org/en-US/docs/Web/API/RTCErrorEvent) 。此错误将是 `dtls-failure` 或 `fingerprint-failure` 。

[`gatheringstatechange`](https://developer.mozilla.org/en-US/docs/Web/API/RTCIceTransport/gatheringstatechange_event "gatheringstatechange")

[`RTCIceTransport`](https://developer.mozilla.org/en-US/docs/Web/API/RTCIceTransport) 的收集状态已更改。

[`icecandidate`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection/icecandidate_event "icecandidate")

每当本地设备识别出一个新的 ICE 候选需要通过调用 [`setLocalDescription()`](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection/setLocalDescription "setLocalDescription()") 添加到本地对等方时，就会发送的 [`RTCPeerConnectionIceEvent`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnectionIceEvent) 。

[`icecandidateerror`](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection/icecandidateerror_event "icecandidateerror")

表示在收集 ICE 候选时发生错误的 [`RTCPeerConnectionIceErrorEvent`](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnectionIceErrorEvent) 。

[`iceconnectionstatechange`](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection/iceconnectionstatechange_event "iceconnectionstatechange")

在其 ICE 连接的状态（可通过 [`iceconnectionstate`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection/iceConnectionState "iceconnectionstate") 属性访问）更改时，会发送给 [`RTCPeerConnection`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection) 。

[`icegatheringstatechange`](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection/icegatheringstatechange_event "icegatheringstatechange")

在其 ICE 收集状态（可通过 [`icegatheringstate`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection/iceGatheringState "icegatheringstate") 属性访问）更改时，会发送给 [`RTCPeerConnection`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection) 。

[`message`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDataChannel/message_event "message")

在数据通道上收到消息。该事件的类型为 [`MessageEvent`](https://developer.mozilla.org/zh-CN/docs/Web/API/MessageEvent) 。

[`negotiationneeded`](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection/negotiationneeded_event "negotiationneeded")

通知 `RTCPeerConnection` 需要通过调用 [`createOffer()`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection/createOffer "createOffer()") ，然后是 [`setLocalDescription()`](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection/setLocalDescription "setLocalDescription()") 来执行会话协商。

[`open`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDataChannel/open_event "open")

`RTCDataChannel` 的底层数据传输已成功打开或重新打开。

[`selectedcandidatepairchange`](https://developer.mozilla.org/en-US/docs/Web/API/RTCIceTransport/selectedcandidatepairchange_event "selectedcandidatepairchange")

`RTCIceTransport` 上的当前选择的 ICE 候选对已更改时触发的事件。

[`track`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection/track_event "track")

当成功协商了媒体流的流式传输后，将向 [`RTCPeerConnection`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection) 添加新轨道时，会发送类型为 [`RTCTrackevent`](https://developer.mozilla.org/en-US/docs/Web/API/RTCTrackEvent) 的 `track` 事件。

[`signalingstatechange`](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection/signalingstatechange_event "signalingstatechange")

在其 [`signalingstate`](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection/signalingState "signalingstate") 更改时，会发送到对等连接。这是由于调用 [`setLocalDescription()`](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection/setLocalDescription "setLocalDescription()") 或 [`setRemoteDescription()`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection/setRemoteDescription "setRemoteDescription()") 引起的。

`statechange`

`RTCDtlsTransport` 的状态已更改。

[`statechange`](https://developer.mozilla.org/en-US/docs/Web/API/RTCIceTransport/statechange_event "statechange")

`RTCIceTransport` 的状态已更改。

[`statechange`](https://developer.mozilla.org/en-US/docs/Web/API/RTCSctpTransport/statechange_event "statechange")

`RTCSctpTransport` 的状态已更改。

[`rtctransform`](https://developer.mozilla.org/en-US/docs/Web/API/DedicatedWorkerGlobalScope/rtctransform_event "rtctransform")

编码的视频或音频帧已准备好在 worker 中使用转换流进行处理。

#### 类型

[`RTCSctpTransport.state`](https://developer.mozilla.org/en-US/docs/Web/API/RTCSctpTransport/state)

指示 [`RTCSctpTransport`](https://developer.mozilla.org/en-US/docs/Web/API/RTCSctpTransport) 实例的状态。

这些 API 用于管理用户身份和安全性，以便为连接认证用户。

`RTCIdentityProvider`

使用户代理能够请求生成或验证身份断言。

[`RTCIdentityAssertion`](https://developer.mozilla.org/en-US/docs/Web/API/RTCIdentityAssertion)

表示当前连接的远程对等方的身份。如果尚未设置和验证对等方，则此接口返回 `null` 。一旦设置，便无法更改。

`RTCIdentityProviderRegistrar`

注册身份提供商（IdP）。

[`RTCCertificate`](https://developer.mozilla.org/en-US/docs/Web/API/RTCCertificate)

表示 [`RTCPeerConnection`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection) 用于身份验证的证书。

#### 接口

[`RTCDTMFSender`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDTMFSender)

管理 [`RTCPeerConnection`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCPeerConnection) 的双音多频（DTMF）信号的编码和传输。

[`RTCDTMFToneChangeEvent`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDTMFToneChangeEvent)

由 [`tonechange`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCDTMFSender/tonechange_event "tonechange") 事件使用，指示 DTMF 音调已开始或结束。此事件不冒泡（除非另有说明），也不可取消（除非另有说明）。

#### 事件

[`tonechange`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCDTMFSender/tonechange_event "tonechange")

连接上已开始播放新的 DTMF 音调，或者 `RTCDTMFSender` 的 [`toneBuffer`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDTMFSender/toneBuffer "toneBuffer") 中的最后一个音调已发送，且缓冲区现在为空。事件的类型为 [`RTCDTMFToneChangeEvent`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDTMFToneChangeEvent) 。

这些接口和事件用于使用在 worker 中运行的转换流处理传入和传出的编码视频和音频帧。

#### 接口

[`RTCRtpScriptTransform`](https://developer.mozilla.org/en-US/docs/Web/API/RTCRtpScriptTransform)

用于将 worker 中运行的转换流插入 RTC 流水线的接口。

[`RTCRtpScriptTransformer`](https://developer.mozilla.org/en-US/docs/Web/API/RTCRtpScriptTransformer)

`RTCRtpScriptTransform` 的 worker 对应项，传递来自主线程的选项，以及可用于通过 [`TransformStream`](https://developer.mozilla.org/zh-CN/docs/Web/API/TransformStream) 传递编码帧的可读流和可写流。

[`RTCEncodedVideoFrame`](https://developer.mozilla.org/en-US/docs/Web/API/RTCEncodedVideoFrame)

表示要在 RTC 流水线中进行转换的编码视频帧。

[`RTCEncodedAudioFrame`](https://developer.mozilla.org/en-US/docs/Web/API/RTCEncodedAudioFrame)

表示要在 RTC 流水线中进行转换的编码音频帧。

#### 属性

[`RTCRtpReceiver.transform`](https://developer.mozilla.org/en-US/docs/Web/API/RTCRtpReceiver/transform)

用于将转换流插入接收器流水线以处理传入的编码视频和音频帧的属性。

[`RTCRtpSender.transform`](https://developer.mozilla.org/en-US/docs/Web/API/RTCRtpSender/transform)

用于将转换流插入发送器流水线以处理传出的编码视频和音频帧的属性。

#### 事件

[`rtctransform`](https://developer.mozilla.org/en-US/docs/Web/API/DedicatedWorkerGlobalScope/rtctransform_event "rtctransform")

RTC 转换已准备好在 worker 中运行，或者已准备处理编码视频或音频帧时触发的事件。

[WebRTC 协议简介](https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API/Protocols)

本文介绍了构建 WebRTC API 的底层协议。

[WebRTC 连接](https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API/Connectivity)

介绍了 WebRTC 连接的工作原理以及如何将各种协议和接口结合起来以构建功能强大的通信应用程序的指南。

[WebRTC 会话的生命周期](https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API/Session_lifetime)

WebRTC 允许你在浏览器应用程序中构建任意数据、音频或视频（或其中的任意组合）的点对点通信。在本文中，我们将从建立连接一直到关闭连接（不再需要时）的整个 WebRTC 会话生命周期进行探讨。

[建立连接：完美协商模式](https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API/Perfect_negotiation)

**完美协商** 是一种建议你的信令流程遵循的设计模式，它在协商过程中提供了透明性，同时允许双方都是提供方或应答方，而无需进行大量的编码来区分两者。

[信令和双向视频通话](https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API/Signaling_and_video_calling)

它将一个之前的示例创建的、基于 WebSocket 的聊天系统转换为教程和示例，并添加了参与者之间进行视频通话的支持。聊天服务器的 WebSocket 连接用于 WebRTC 信令。

[WebRTC 使用的编解码器](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Formats/WebRTC_codecs)

介绍了 WebRTC 需要浏览器支持的编解码器以及各种流行浏览器支持的可选编解码器。本文还包括了一个帮助你选择最适合你需求的编解码器的指南。

[使用 WebRTC 数据通道](https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API/Using_data_channels)

本指南介绍了如何使用对等连接和相关的 [`RTCDataChannel`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCDataChannel) 在两个对等方之间交换任意数据。

[在 WebRTC 中使用 DTMF](https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API/Using_DTMF)

WebRTC 与连接到老式电话系统的网关进行交互的支持，包括使用 [`RTCDTMFSender`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDTMFSender) 接口发送 DTMF 音调。本指南展示了如何做到这一点。

[使用 WebRTC 编码转换](https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API/Using_Encoded_Transforms)

本指南展示了一个 Web 应用程序如何使用在 worker 中运行的 [`TransformStream`](https://developer.mozilla.org/zh-CN/docs/Web/API/TransformStream) 来修改传入和传出的 WebRTC 编码视频和音频帧。

[使用 WebRTC adapter.js 改善兼容性](https://developer.mozilla.org/zh-CN/docs/Web/API/#%E4%BA%92%E6%93%8D%E4%BD%9C%E6%80%A7)

WebRTC 组织 [在 GitHub 上提供了 WebRTC 适配器](https://github.com/webrtc/adapter/) 来解决不同浏览器的 WebRTC 实现的兼容性问题。适配器是一个 JavaScript 垫片，使你的代码只需按照规范编写，便可以在所有支持 WebRTC 的浏览器中“正常工作”。

[RTCDataChannel 简单示例](https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API/Simple_RTCDataChannel_sample)

[`RTCDataChannel`](https://developer.mozilla.org/zh-CN/docs/Web/API/RTCDataChannel) 接口是一个允许你在两个对等方之间打开一个通道（通过该通道你可以发送和接收任意数据）的特性。API 故意设计成与 [WebSocket API](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSockets_API) 类似，以便可以使用相同的编程模型。

[使用 Peer.js 构建联网电话](https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API/Build_a_phone_with_peerjs)

本教程将逐步指导你如何使用 Peer.js 构建电话。

| Specification |
| --- |
| [WebRTC: Real-Time Communication in Browsers](https://w3c.github.io/webrtc-pc/) |
| [Media Capture and Streams](https://w3c.github.io/mediacapture-main/) |
| [Media Capture from DOM Elements](https://w3c.github.io/mediacapture-fromelement/) |

- [Web 实时通信的应用层协议协商](https://datatracker.ietf.org/doc/rfc8833/)
- [WebRTC 音频编解码器和处理要求](https://datatracker.ietf.org/doc/rfc7874/)
- [RTCWeb 数据通道](https://datatracker.ietf.org/doc/rfc8831/)
- [RTCWeb 数据通道协议](https://datatracker.ietf.org/doc/rfc8832/)
- [Web 实时通信（WebRTC）：媒体传输和 RTP 的使用](https://datatracker.ietf.org/doc/rfc8834/)
- [WebRTC 安全架构](https://datatracker.ietf.org/doc/rfc8827/)
- [RTCWEB 的传输](https://datatracker.ietf.org/doc/rfc8835/)

- [交互式连接建立（ICE）：用于网络地址转换器（NAT）穿越的协议](https://datatracker.ietf.org/doc/html/rfc5245)
- [NAT 会话穿越应用程序（STUN）](https://datatracker.ietf.org/doc/html/rfc5389)
- [NAT 会话穿越应用程序（STUN）的 URI 方案协议](https://datatracker.ietf.org/doc/html/rfc7064)
- [绕过 NAT 的中继穿越（TURN）的统一资源标识符](https://datatracker.ietf.org/doc/html/rfc7065)
- [具有会话描述协议（SDP）的提供/应答模型](https://datatracker.ietf.org/doc/html/rfc3264)
- [NAT 会话穿越应用程序（STUN）的第三方认证扩展](https://datatracker.ietf.org/doc/rfc7635/)

- [`MediaDevices`](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaDevices)
- [`MediaStreamEvent`](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaStreamEvent)
- [`MediaStreamTrack`](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaStreamTrack)
- [`MessageEvent`](https://developer.mozilla.org/zh-CN/docs/Web/API/MessageEvent)
- [`MediaStream`](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaStream)
- [媒体捕捉与媒体流 API](https://developer.mozilla.org/zh-CN/docs/Web/API/Media_Capture_and_Streams_API)
- [Firefox 多流和 Jitsi Videobridge 的重新协商](https://hacks.mozilla.org/2015/06/firefox-multistream-and-renegotiation-for-jitsi-videobridge/)
- [使用 SocketPeer 拨开 WebRTC 迷雾](https://hacks.mozilla.org/2015/04/peering-through-the-webrtc-fog-with-socketpeer/)
- [Party Bus 内部：构建具有多个实时视频流和交互式图形的 Web 应用](https://hacks.mozilla.org/2014/04/inside-the-party-bus-building-a-web-app-with-multiple-live-video-streams-interactive-graphics/)
- [Web 媒体技术](https://developer.mozilla.org/zh-CN/docs/Web/Media)