===================
配置指南
===================

Kurento通过精心策划的一整套技术来共同合作的。其中一些技术可以接受不同的配置参数使得Kurento通过下面的多个配置文件可用:

- ``/etc/kurento/kurento.conf.json``:主配置文件。提供Kurento Media Server本身行为的设置.
- ``/etc/kurento/modules/kurento/MediaElement.conf.ini``: 各种 *MediaElement*的通用参数.
- ``/etc/kurento/modules/kurento/SdpEndpoint.conf.ini``: *SdpEndpoint* 的音频/视频参数 (即 *WebRtcEndpoint* 和 *RtpEndpoint*).
- ``/etc/kurento/modules/kurento/WebRtcEndpoint.conf.ini``: *WebRtcEndpoint* 的具体参数.
- ``/etc/kurento/modules/kurento/HttpEndpoint.conf.ini``: *HttpEndpoint* 的具体参数.
- ``/etc/default/kurento-media-server``: 该文件由系统服务初始化文件加载。定义了一些环境变量, 这些变量会对发生奔溃时生成的调试日志或核心转储文件等功能产生影响.



Media Server
============

File: ``/etc/kurento/kurento.conf.json``.

.. code-block:: text
   :emphasize-lines: 8

	{
	  "mediaServer" : {
	    "resources": {
	    //  // 当请求创建一个对象时，如果资源用量达到下面的阈值，抛出异常
	    //  "exceptionLimit": "0.8",
	    //  // 如果没有任何存活对象，且资源用量达到下面的阈值，则重启服务器
	    //  "killLimit": "0.7",
	        // 垃圾回收器活动间隔（秒）
	        "garbageCollectorPeriod": 240
	    },
	    "net" : {
	    // WS用于Kurento Protocol
	      "websocket": {
	      // 普通WS端口
	        "port": 8888,
	        // WSS端口、数字证书信息
	        //"secure": {
	        //  "port": 8433,
	        //  "certificate": "defaultCertificate.pem",
	        //  "password": ""
	        //},
	        //"registrar": {
	        //  "address": "ws://localhost:9090",
	        //  "localAddress": "localhost"
	        //},
	        // URL路径
	        "path": "kurento",
	        "threads": 10
	      }
	    }
	  }
	}




MediaElement
============

File: ``/etc/kurento/modules/kurento/MediaElement.conf.ini``.

.. code-block:: text
   :emphasize-lines: 8

	;outputBitrate=1500000 // 输出比特率值



SdpEndpoint
===========

File: ``/etc/kurento/modules/kurento/SdpEndpoint.conf.json``.

.. code-block:: text
   :emphasize-lines: 8

	{
		"numAudioMedias" : 1,
		"numVideoMedias" : 1,
		"audioCodecs" : [
	    {
	      "name" : "opus/48000/2",
		// 接下来是关于如何配置编解码器的示例.
		// WARNING: 该使用属性暂时未被支持
		      "properties" : {
		        "maxcodedaudiobandwidth" : "16000",
		        "maxaveragebitrate" : "20000",
		        "stereo": "0",
		        "useinbandfec" : "2",
		        "usedtx" : "0"
		      }
		    },
		    {
		      "name" : "PCMU/8000"
		    },
		    {
		      "name" : "AMR/8000"
		    }
	    ],
		"videoCodecs" : [
		{
		  "name" : "VP8/90000"
		},
		{
		  "name" : "H264/90000"
		}
		]
	}





WebRtcEndpoint
==============

File: ``/etc/kurento/modules/kurento/WebRtcEndpoint.conf.ini``.

.. code-block:: text
   :emphasize-lines: 8

	; 仅支持IP地址，不支持地址的域名
	; 你必须找到一个有效的stun服务器。 你可以检查它是否有效
	; 使用此工具:
	;   http://webrtc.github.io/samples/src/content/peerconnection/trickle-ice/
	; stunServerAddress=<serverAddress>
	; stunServerPort=<serverPort>

	; turnURL 为WebRTC提供配置所需的TURN信息 .
	;    'address' 必须是 IP (非域名).
	;    'transport' 是可选的 (默认为UDP).
	; turnURL=user:password@address:port(?transport=[udp|tcp|tls])

	;pemCertificate 弃用. 使用 pemCertificateRSA 替代
	;pemCertificate=<path>
	;pemCertificateRSA=<path>
	;pemCertificateECDSA=<path>





HttpEndpoint
============

File: ``/etc/kurento/modules/kurento/HttpEndpoint.conf.ini``.

.. code-block:: text
   :emphasize-lines: 8

	serverAddress=localhost
	port=9091

	; 在服务器需求等情况下，开放的IP Addess可能会有所帮助
	; 向主机名不同的客户端提供URL
	; 服务器正在监听。如果未提供此选项，http服务器将继续尝试
	; 查找系统中的任何可用地址。

	; announcedAddress=localhost





Debug Logging
=============

File: ``/etc/default/kurento-media-server``.

初始化时自动加载


Service Init
============

*kurento-media-server*包提供了一个与Ubuntu系统集成的服务文件. 这个文件从 */etc/default/kurento-media-server* 加载用户自定义配置参数, 用户可以根据需要配置多个功能.
