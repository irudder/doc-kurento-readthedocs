===========
Kurento API
===========

.. contents:: 目录

**Kurento媒体服务**可以通过暴露API的方式来控制, 以便于开发者通过高级语言与它相交互. Kurento项目已经为多个平台提供了该API的 :doc:`Kurento Client </features/kurento_client>` 实现方式.

如果你需要使用其他没被已提供的高级语言，你可以使用 :doc:`Kurento Protocol</features/kurento_protocol>` 来实现自己的Kurento client, kurento协议是基于 :term:`WebSocket` 和 :term:`JSON-RPC`.

下面的教程将介绍Kurento Api的高级使用方式，关于kurento媒体服务向客户端暴露的媒体能力，如果你是需要查阅kurento的可运行demo,请参考 :doc:`教程摘录 </user/tutorials>`.



媒体元素与媒体管道
==================================

Kurento是基于两种概念作为积木向应用开发人员展示的:

- **媒体元素**. 媒体元素是对媒体流执行特定操作的功能单元，媒体元素是一种将每种功能的方式表示为应用程序开发人员的独立“黑盒子”（媒体元素），他不需要了解元素的底层细节而方便使用它。媒体元素能够从其他元素（通过媒体源）*接收* 媒体并将媒体 *发送* 到其他元素（通过媒体接收器）。 基于它们功能的不通,媒体元素可以被分析不同的组：

  - **输入端**: 媒体元素可以接收媒体流和将其注入媒体管道。这里有几种不同方式的输入端，文件输入是指从文件中获取媒体流，网络输入是指从网络中获取媒体流,捕获端输入是指通过相机获取其他硬件资源来截取媒体流.
  - **过滤器**: 负责转换或分析媒体的媒体元素. 因此它可以执行混流，复用，分析，扩展等过滤操作.
  - **中心枢纽**: 媒体对象通过管道管理多媒体流，一个 *中心* 包含不同的 *中心点* 以便于其他媒体元素互联，取决于中心的类型，有不同的方式来控制媒体流。例如，有一个叫 *Composite* 的中心合并所有的输入流来合并成唯一的输出视频流，所有输入都排列成网格.
  - **输出端**: 体元素能够从媒体管道中取出媒体流。同样，有几种类型的输出端点，专门用于文件，网络，屏幕等。

- **媒体管道**: 媒体管道是媒体元素通道, 其中源元素生成的输出流被馈送到一个或多个宿元素中。 因此， 管道表示能够在流上执行一系列操作的"pipe"。

  .. figure:: /images/kurento-java-tutorial-2-magicmirror-pipeline.png
     :align:  center
     :alt:    Media Pipeline example

     *实现从WebRtcEndpoint接收媒体流的交互式多媒体应用程序的媒体管道示例，在检测到的面部上叠加和图像并发送回结果流*

Kurento API 是 :wikipedia:`面向对象编程 <Object-oriented programming>`. 这意味着这些类都可以被实例化为队形，这些对象提供的 *属性* 代表kurento服务的网络状态，暴露出的 *方法* 可以被服务所执行.

下面的类图展示了Kurento Api主要之间的关系:

.. graphviz:: /images/graphs/mediaobjects.dot
   :align: center
   :caption: 主要类的类图



Endpoints
=========

 **WebRtcEndpoint** 是通过web进行流媒体实时通讯(RTC)的输入/输出端. 它基于浏览器实现了 :term:`WebRTC` 技术.

.. image:: /images/toolbox/WebRtcEndpoint.png
   :align:  center

 **RtpEndpoint** 是一个与远程网络通过 :term:`RTP` 协议进行双向内容传输的输入/输出端.它使用 :term:`SDP` 作为媒体内容校验.

.. image:: /images/toolbox/RtpEndpoint.png
   :align:  center

 **HttpPostEndpoint** 是一个输入端点，它使用HTTP POST请求（如HTTP文件上载功能）接受媒体流。

.. image:: /images/toolbox/HttpPostEndpoint.png
   :align:  center

 **PlayerEndpoint** 是一个输入端，它从文件系统，HTTP URL或RTSP URL中检索内容并将其注入媒体管道。.

.. image:: /images/toolbox/PlayerEndpoint.png
   :align:  center

A **RecorderEndpoint** 是一个输出端点，提供以可靠模式存储内容的功能（不丢弃数据）.它包含用于音频和视频的媒体 ``媒体接收器`` .

.. image:: /images/toolbox/RecorderEndpoint.png
   :align:  center

以下类图显示了主要endpoint类的关系：

.. graphviz:: /images/graphs/endpoints.dot
   :align: center
   :caption: Class diagram of main Endpoints in Kurento API



Filters
=======

过滤器是一个可执行媒体处理,计算机视觉,增强现实等的媒体元素。

 **ZBarFilter** 过滤器检测视频流中的二维码和条形码.当发现对应的条码时,过滤器会触发一个 ``CodeFoundEvent`` 事件. 客户端可以向此事件添加侦听器以执行某些操作.

.. image:: /images/toolbox/ZBarFilter.png
   :align:  center

 **FaceOverlayFilter** 过滤器可以识别视频流中的脸部并用可配置的图像对其进行覆盖处理.

.. image:: /images/toolbox/FaceOverlayFilter.png
   :align:  center

**GStreamerFilter** 是一个通用的过滤器接口，允许任何GStreamer元素注入到Kurento媒体管道. 但请注意当前GStreamerFilter仅支持单个元素的注入，同一时间不能超过一个; 如果你需要同时注入多个元素可以使用多个GStreamerFilters.

.. image:: /images/toolbox/GStreamerFilter.png
   :align:  center

以下类图显示了主要过滤器类间的关系:

.. graphviz:: /images/graphs/filters.dot
   :align: center
   :caption: Class diagram of main Filters in Kurento API



Hubs 
====

Hubs是负责管理管道中多个媒体流的媒体对象。它有几个中心端口，其他媒体元素连接在一起.

**Composite**混合了其连接输入的音频流，并构建了一个网格与它们的视频流的枢纽.

.. image:: /images/toolbox/Composite.png
   :align:  center

**DispatcherOneToMany** 将给定的输入发送到所有连接的输出HubPort.

.. image:: /images/toolbox/DispatcherOneToMany.png
   :align:  center

**Dispatcher** 允许在任意输入输出HubPort对之间进行路由。.

.. image:: /images/toolbox/Dispatcher.png
   :align:  center

以下类图显示了hubs的关系:

.. graphviz:: /images/graphs/hubs.dot
   :align: center
   :caption: Class diagram of main Hubs in Kurento API
