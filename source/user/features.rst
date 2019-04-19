========
功能模块
========

本页概述了Kurento提供的功能，以及指向最重要的文档页面的链接.



Kurento API, 客户端, 协议
==================================

Kurento Media Server 通过 :doc:`Kurento API </features/kurento_api>` 的RPC接口暴露了所有功能. 任何使用兼容json的客户端都可以直接使用这个API，但是推荐的方式是使用 :doc:`Kurento Client </features/kurento_client>` 库; 目前提供的库有 *Java*, *Browser Javascript*, 和 *Node.js*.

如果你想使用其他程序语言，你可以基于 *WebSocket* and *JSON-RPC* 按照 :doc:`Kurento 协议 </features/kurento_protocol>` 规范来编写自定义客户端程序.

下面图片展示了在下列三种场景中如何使用Kurento 客户端:

- 在兼容 WebRTC 的浏览器中直接使用Kurento JavaScript Client.
- 在Java EE 应用服务中使用Kurento Java Client.
- 在Node.js 服务中使用Kurento JavaScript Client.

.. figure:: /images/kurento-clients-connection.png
   :align: center
   :alt: Connection of Kurento Clients (Java and JavaScript) to Kuento Media Server

   *连接Kurento Clients (Java and JavaScript) 与 Kuento Media Server*

这里有三种技术场景的完整案例 :doc:`部分教程 </user/tutorials>`.

Kurento Client API基于 **媒体元素** 的概念. 媒体元素拥有特定的媒体功能. 例如,媒体元素 *WebRtcEndpoint* 拥有发送和接收WebRTC媒体流的能力; 媒体元素 *RecorderEndpoint* 拥有录制接收的媒体流存如文件系统的能力; *FaceOverlayFilter* 检测交换的视频流上的面部并在其上添加特定的重叠图像, 等等. Kurento 公开了丰富的媒体元素工具箱作为其API的一部分.

.. figure:: /images/kurento-basic-toolbox.png
   :align: center
   :alt: Some Media Elements provided out of the box by Kurento

   *一些媒体元素可以通过Kurento开箱即用*

理解这些概念可以通过查阅 :doc:`/features/kurento_api` 和 :doc:`/features/kurento_protocol`. 当然你还可以通过查阅已经实现的API参考文档: :doc:`/features/kurento_client`.



Kurento 模块
===============

Kurento被设计成是一个可插拔的框架. Kurento 媒体服务将 *kms-core*, *kms-elements* 和 *kms-filters* 作为默认模块.

此外，还有其他内置模块可以增强Kurento Media Server提供的功能。这些模块分别是 *kms-crowddetector*, *kms-pointerdetector*, *kms-chroma*, 和 *kms-platedetector*.

最后, Kurento媒体服务可以通过自定义模块来扩展.

.. figure:: ../images/kurento-modules01.png
   :align:  center
   :alt:    Kurento modules architecture

   *Kurento模块架构。 Kurento Media Server可以使用内置模块（crowddetector，pointerdetector，chroma，platedetector）以及其他自定义模块进行扩展.*

获取更多信息可查阅 :doc:`/features/kurento_modules`.



RTP 流
=============

除了WebRTC连接，Kurento Media Server还能够管理标准RTP流，允许将KMS实例连接到各种设备.

处理RTP连接时需要注意两个点：KMS实现的自动拥塞控制算法 (阅 :ref:`features-remb`), 和 NAT 遍历功能 (阅 :doc:`/features/nat_traversal`).



.. _features-remb:

拥塞控制 / REMB
=========================

Kurento 是基于 *Google Congestion Control* 算法实现的, 所以它能够生成并解析 ``abs-send-time`` RTP 头信息 和 :term:`REMB` RTCP 消息.

通过在SDP Offer船体媒体级属性 ``goog-remb`` 来校验通过 . 案例:

.. code-block:: text
   :emphasize-lines: 8

   v=0
   o=- 0 0 IN IP4 127.0.0.1
   s=-
   c=IN IP4 127.0.0.1
   t=0 0
   m=video 5004 RTP/AVPF 103
   a=rtpmap:103 H264/90000
   a=rtcp-fb:103 goog-remb
   a=sendonly
   a=ssrc:112233 cname:user@example.com

``a=rtcp-fb`` 是 *RTCP 反馈* 能力属性, 如定义 :rfc:`4585`.

KMS在连接的发送方和接收方之间实现REMB传播.这意味着当KMS用作视频发送方与一个或多个视频接收方之间的代理时，来自接收方的最小REMB值将被转发给发送方. 这允许发送方选择较低的比特率，以容纳在另一侧连接到KMS的所有接收方.

有关什么是REMB及其如何适应RMCAT更大项目的更多背景信息，请阅读我们的知识库文档: :doc:`/knowledge/congestion_rmcat`.
