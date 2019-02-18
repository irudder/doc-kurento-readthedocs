========================
关于 Kurento 和 WebRTC
========================

:term:`Kurento` 是关于 :term:`WebRTC` 的媒体服务, 为简化web端和一段的视音频的应用而开发出一套apis接口. 它的功能点包括组通信,转码,录制,流合成,广播与视音频流路由.

Kurento 提供一个多媒体框架利用一下功能易于创建一个多媒体应用的任务:

- **动态 WebRTC 媒体管道**: Kurento 允许用户自定义 :ref:`媒体管道 <writing-app-pipelines>` 连接像浏览器与移动端app这样的基于Webrtc的对等网络. 这些媒体管道基于可组合元素，例如播放器，录音机，混音器等，可以在任何时间点混合和匹配，激活或停用，即使媒体已经流动.

- **Client/Server 构建**: 使用Kurento开发的应用程序遵循客户端/服务器的基本架构. Kurento Media Server (KMS) 服务实现了Kurento Protocol(协议)从而提供了一套基于 WebSocket的接口,允许客户端应用程序自定义拓扑管道.

- **Java and JavaScript 客户端应用**: KMS部署的典型用例包括三层体系结构，其中用户的浏览器通过中间客户端应用程序与KMS服务器交互。有几个官方的Kurento客户端库，支持在客户端应用程序中使用Java和JavaScript。可以使用WebSocket协议轻松实现其他语言的客户端。

- **第三方模块**:Kurento Media Server 具有基于插件的可扩展体系结构, 允许第三方实现可添加到其媒体管道的模块.  这允许将媒体处理算法集成到任何WebRTC应用程序，例如集成计算机视觉，增强现实，视频索引和语音分析. 这些拓展都需要的是创建一个新的Kurento元素，并可以在任何已经存在的媒体管道中使用。

本文档包含有关如何成为KMS开发人员的高级说明。 Kurento客户端应用程序的开发不在本文档的讨论范围之内，此处不再赘述。

Kurento Media Server的代码是开源的， 根据 `Apache License Version 2.0`_ 发布并可以通过 `GitHub 获取`_.

.. _Apache License Version 2.0: https://www.apache.org/licenses/LICENSE-2.0
.. _available on GitHub: https://github.com/Kurento



WebRTC 媒体 服务
====================

`WebRTC <https://webrtc.org/>`__ 是一组协议，机制和API，通过对等连接为浏览器和移动应用程序提供实时通信（RTC）功能。它被认为是一种允许浏览器直接通信而无需任何类型的基础设施调解的技术，这个模型足以创建基本的应用程序，但在它上面很难实现像组通信、媒体流录制、媒体广播或媒体转换等功能。出于这个原因，许多应用程序需要使用媒体服务器。

.. figure:: /images/media-server-intro.png
   :align: center
   :alt: Peer-to-peer WebRTC approach vs. WebRTC through a media server

   *WebRTC点对点方法 vs. WebRTC通过媒体服务器*

从概念上讲，WebRTC媒体服务知识一个多媒体中间件用于媒体流从源流向目的地。

媒体服务能够处理传入的媒体流并提供不同的结果，例如:

- 组通信:在几个接收器之间分配一个对等体生成的媒体流，即充当多会议单元（"MCU"”"）。
- 混合: 将多个传入流转换为单个复合流.
- 转码:在不兼容的客户端之间即时适应编解码器和格式.
- 录制: 以持久的方式存储媒体在其他端进行交换.

.. figure:: /images/media-server-capabilities.png
   :align: center
   :alt: Typical WebRTC Media Server capabilities

   *典型的WebRTC媒体服务功能*



Kurento Media Server
====================

Kurento的主要组件是 **Kurento媒体服务器** (KMS) ， 负责媒体传输，处理，录制和播放。 KMS建立在神奇的 :term:`GStreamer` 多媒体库之上，并提供以下功能：

-  网络流媒体协议, 包括 :term:`HTTP`, :term:`RTP` and :term:`WebRTC`.
-  支持媒体混合和媒体路由/调度的组通信（MCU和SFU功能）.
-  对实现计算机视觉和增强现实算法的过滤器的通用支持.
-  媒体存储，支持 :term:`WebM` 和 :term:`MP4` 的写入操作，并以 *GStreamer* 支持的所有格式播放
Media storage that supports writing operations for :term:`WebM` and :term:`MP4` and playing in all formats supported by *GStreamer*.
-  GStreamer支持的任何编解码器之间的自动媒体转码, 包括 VP8, H.264, H.263, AMR, OPUS, Speex, G.711 等等.

.. figure:: /images/kurento-media-server-intro.png
   :align: center
   :alt: Kurento Media Server capabilities

   *Kurento Media Server 功能*



Kurento 设计 原则
=========================

Kurento基于一下几条主要原则设计的:

    **单独的媒体和信令平面**
        :term:`信令 <signaling plane>` and :term:`媒体 <media plane>` 是两个独立的平面，Kurento的设计使应用程序可以分别单独处理这些多媒体进程.

    **媒体和应用服务的分发**
        Kurento媒体服务器和应用程序可以在不同的计算机之间并置，升级或分布式部署.

       单个应用程序可以调用多个Kurento Media Server 的服务。相反的情况也适用，即Kurento Media Server 可以参加多个应用程序的请求。.

    **云端可扩展**
        Kurento适合集成到云环境中，充当PaaS（平台即服务）组件.

    **媒体管道**
        链接 :term:`媒体元素 <Media Element>` via :term:`媒体管道 <Media Pipeline>` 是一种挑战多媒体处理复杂性的直观方法.

    **应用开发**
        发人员无需了解内部Kurento Media Server的复杂性：所有应用程序都可以部署在开发人员喜欢的任何技术或框架中，从客户端到服务器，从浏览器到云服务.

    **端到端通信能力**
        Kurento提供端到端通信功能，因此开发人员无需处理客户端设备上传输，编码/解码和渲染媒体的复杂性.

    **完全可处理的媒体流**
      Kurento不仅可以实现交互式人际通信（例如，具有Skype的会话呼叫推送/接收功能）, 但也是人对机（例如通过实时流传输的视频点播）和机器对机器（例如远程视频记录，多感官数据交换）通信.

    **媒体的模块化处理**
       通过 :term:`媒体元素 <Media Element>` 和 :term:`管道 <Media Pipeline>` 实现的模块化，可以通过“面向图形”的语言来定义应用程序的媒体处理功能，应用程序开发人员能够通过链接适当的功能来创建所需的逻辑。

    **审计处理**
        Kurento能够为QoS监控，计费和审计生成丰富而详细的信息。

    **无缝集成IMS**
        Kurento旨在支持无缝集成到Telephony Carriers的 :term:`IMS` 基础架构中。

    **透明媒体适应层**
        Kurento提供透明的媒体适配层，以使得在屏幕尺寸，功耗，传输速率等方面具有不同要求的不同设备之间的会聚成为可能。
