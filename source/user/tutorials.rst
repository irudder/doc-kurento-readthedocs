=================
Kurento 教程
=================

本教程向我们展示了怎样使用Kurento框架来实现一个不同类型的 :term:`WebRTC` 多媒体议用. 教程展示了三种形式:

- **Java**: 展示了客户端与基于 *Spring Boot*的应用程序相互作用, 逻辑编排在客户端与Kurento媒体服务通讯的过程中.

- **Browser JavaScript**: 应用程序通过浏览器直接与Kurento媒体服务交流. 这里的逻辑教程直接通过浏览器编写, 于是,不需要应用程序服务器.

- **Node.js**:客户端与使用Node.js技术制作的应用程序服务器进行交互. 应用程序保持逻辑编排客户端间的通讯并控制他们的Kurento媒体服务能力.

.. 备注::

   这些教程是根据学习目标创建的。它们不适用于可能出现不同非管理错误情况的生产环境。
   **使用风险自负！**

.. 备注::

   在使用WebRTC时本教程要求必须使用 ``HTTPS`` . 根据说明将会提供进一步关于如何使得应用程序安全的更多信息.



Hello World
===========

使用Kurento你可以创建一个简单的WebRTC应用. 它是吸纳了 :term:`WebRTC` *回放* ( WebRTC媒体流通过客户端传输到Kurento媒体服务再返回到客户端展示)

.. toctree::
   :maxdepth: 1

   Java </tutorials/java/tutorial-helloworld>
   Browser JavaScript </tutorials/js/tutorial-helloworld>
   Node.js </tutorials/node/tutorial-helloworld>



WebRTC 魔镜
===================

这个web应用包含了一个 :term:`WebRTC` *环回* 的视频交流, 在检测到面部的时候增加一个有趣的帽子. 这是关于计算机视觉和增强现实过滤器的案例.

.. toctree::
   :maxdepth: 1

   Java </tutorials/java/tutorial-magicmirror>
   Browser JavaScript </tutorials/js/tutorial-magicmirror>
   Node.js </tutorials/node/tutorial-magicmirror>



RTP 接收器
============

此Web应用程序显示接收传入的RTP或SRTP流，并通过WebRTC连接进行回放。

.. toctree::
   :maxdepth: 1

   Java </tutorials/java/tutorial-rtp-receiver>



WebRTC 1对多广播
============================

 :term:`WebRTC` 视频广播. 1个点发送视频流N个点接收它.

.. toctree::
   :maxdepth: 1

   Java </tutorials/java/tutorial-one2many>
   Node.js </tutorials/node/tutorial-one2many>



WebRTC 1对1视频通话
============================

这个web应用时基于 :term:`WebRTC` 的视频会话(1对1通话).

.. toctree::
   :maxdepth: 1

   Java </tutorials/java/tutorial-one2one>
   Node.js </tutorials/node/tutorial-one2one>



WebRTC 使用录音/过滤来进行1对1的视频通话
=========================================================

这是具有视频录制和增强现实功能的One-To-One应用程序的增强版本。

.. toctree::
   :maxdepth: 1

   Java </tutorials/java/tutorial-one2one-adv>



WebRTC 多对多的视频通讯 (群组通讯)
===========================================

本教程将多个参与者连接到同一视频会议. 群组通讯将考虑到 (在媒体服务器端) N*N 个 WebRTC 端点, N标识参与会议的客户都试了.

.. toctree::
   :maxdepth: 1

   Java </tutorials/java/tutorial-groupcall>



Media 元素元数据
=======================

本教程将检测和绘制在网络视屏摄像头中的脸部. 它连接的过滤器: KmsDetectFaces 和 the KmsShowFaces.

.. toctree::
   :maxdepth: 1

   Java </tutorials/java/tutorial-metadata>



WebRTC 媒体播放器
===================

本教程将从磁盘中读文件并以Webrtc的方式播放视频.

.. toctree::
   :maxdepth: 1

   Java </tutorials/java/tutorial-player>



WebRTC 传出数据通道
=============================

本教程将视频注入QR(二维码)过滤器，然后将流发送到WebRTC。 QR检测事件通过WebRTC数据通道传送，以在浏览器中显示.

.. toctree::
   :maxdepth: 1

   Java </tutorials/java/tutorial-send-datachannel>



WebRTC 传入数据通道
============================

本教程介绍了如何通过数据通道传送从浏览器发送的文本消息，以便与环回视频一起显示.

.. toctree::
   :maxdepth: 1

   Java </tutorials/java/tutorial-show-datachannel>
   Browser JavaScript </tutorials/js/tutorial-helloworld-datachannels>



WebRTC 录制
================

本教程包含两个部分:

1.  :term:`WebRTC` *环回* 将流写入磁盘.
2. 流回放.

Users can choose which type of media to send and record: audio, video or both.

.. toctree::
   :maxdepth: 1

   Java </tutorials/java/tutorial-recorder>
   Browser Javascript </tutorials/js/tutorial-recorder>



WebRTC 存储库
=================

T本教程类似于录制教程，但是使用存储卡记录元数据.

.. toctree::
   :maxdepth: 1

   Java </tutorials/java/tutorial-repository>



WebRTC 统计分析
=================

本教程实现了 :term:`WebRTC` *环回* 和显示如何收集WebRTC统计信息.

.. toctree::
   :maxdepth: 1

   Browser JavaScript </tutorials/js/tutorial-loopback-stats>
