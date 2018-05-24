title: Android 基本技术点回顾
tags:
  - ''
  - 网络发现
categories:
  - ''
  - Android
author: 侯大帅
date: 2018-05-24 13:39:00
---
### 无线连接设备
>1. Network Service Discovery:NSD网络服务发现。
2. WiFi建立P2P连接
3. 使用WiFi P2P服务

#### NSD网络发现
* NsdManager
>管理涉及到服务注册、监听器、取消服务等，当业务完成要及时取消注册服务(取消注册、关闭搜索服务)，防止占用资源

	- registerService
    		注册监听器
    - resolveService
    		解析监听器
    - discoverServices
    		搜索可发现服务
* NsdServiceInfo
		注册服务的信息
        - setServiceName
        - setPort
        - setServiceType
        不推荐硬编码设置端口号，建议使用这种：
        ServerSocket sock = new ServerSocket(0);  
        port = sock.getLocalPort();  
        sock.close();  
        其中的服务类型写法：指定应用使用的协议和传输层。语法是“_< protocol >._< transportlayer >”
* NsdManager.DiscoveryListener
		mDiscoveryListener = new NsdManager.DiscoveryListener() {

            //  Called as soon as service discovery begins.
            @Override
            public void onDiscoveryStarted(String regType) {
                Log.d(TAG, "Service discovery started");
            }

            @Override
            public void onServiceFound(NsdServiceInfo service) {
                // A service was found!  Do something with it.
                Log.d(TAG, "Service discovery success" + service);
                if (!service.getServiceType().equals("_http._tcp.")) {
                    // Service type is the string containing the protocol and
                    // transport layer for this service.
                    Log.d(TAG, "Unknown Service Type: " + service.getServiceType());
                } else if (service.getServiceName().equals(mServiceName)) {
                    // The name of the service tells the user what they'd be
                    // connecting to. It could be "Bob's Chat App".
                    Log.d(TAG, "Same machine: " + mServiceName);
                } else if (service.getServiceName().contains("NsdChat")){
                    mNsdManager.resolveService(service, mResolveListener);
                }
            }

            @Override
            public void onServiceLost(NsdServiceInfo service) {
                // When the network service is no longer available.
                // Internal bookkeeping code goes here.
                Log.e(TAG, "service lost" + service);
            }

            @Override
            public void onDiscoveryStopped(String serviceType) {
                Log.i(TAG, "Discovery stopped: " + serviceType);
            }

            @Override
            public void onStartDiscoveryFailed(String serviceType, int errorCode) {
                Log.e(TAG, "Discovery failed: Error code:" + errorCode);
                mNsdManager.stopServiceDiscovery(this);
            }

            @Override
            public void onStopDiscoveryFailed(String serviceType, int errorCode) {
                Log.e(TAG, "Discovery failed: Error code:" + errorCode);
                mNsdManager.stopServiceDiscovery(this);
            }
        };
* NsdManager.RegistrationListener
		mRegistrationListener = new NsdManager.RegistrationListener() {

            @Override
            public void onServiceRegistered(NsdServiceInfo NsdServiceInfo) {
                // Save the service name.  Android may have changed it in order to
                // resolve a conflict, so update the name you initially requested
                // with the name Android actually used.
                mServiceName = NsdServiceInfo.getServiceName();
            }

            @Override
            public void onRegistrationFailed(NsdServiceInfo serviceInfo, int errorCode) {
                // Registration failed!  Put debugging code here to determine why.
            }

            @Override
            public void onServiceUnregistered(NsdServiceInfo arg0) {
                // Service has been unregistered.  This only happens when you call
                // NsdManager.unregisterService() and pass in this listener.
            }

            @Override
            public void onUnregistrationFailed(NsdServiceInfo serviceInfo, int errorCode) {
                // Unregistration failed.  Put debugging code here to determine why.
            }
        };
* NsdManager.ResolveListener
		 mResolveListener = new NsdManager.ResolveListener() {

            @Override
            public void onResolveFailed(NsdServiceInfo serviceInfo, int errorCode) {
                // Called when the resolve fails.  Use the error code to debug.
                Log.e(TAG, "Resolve failed" + errorCode);
            }

            @Override
            public void onServiceResolved(NsdServiceInfo serviceInfo) {
                Log.e(TAG, "Resolve Succeeded. " + serviceInfo);

                if (serviceInfo.getServiceName().equals(mServiceName)) {
                    Log.d(TAG, "Same IP.");
                    return;
                }
            }
        };
  大致流程：注册本机服务 -> 注册监听 -> 注解解析 -> 搜索发现服务 -> 解析可发现的 -> 获取ip 端口号 -> 连接
 
 ----
#### WiFi p2p
> p2p不需要访问网络，但是会使用java中的socket，socket需网络权限(  CHANGE_WIFI_STATE、ACCESS_WIFI_STATE、INTERNET)

	首先在本地注册一个服务，服务注册是要将服务类型、服务信息封装到WifiP2pServiceInfo中，再通过WifiP2pManager将服务添加到本地，本地服务添加后注册监听DnsSdTxtRecordListener来实时监听record记录数据，注册DnsSdServiceResponseListener接收服务的实际描述和连接的信息，前者处理记录连接的服务信息用来跟后者做对应关系处理。前者给你一组对他的服务的描述信息，然后在后者进行匹配，将描述信息与实际的硬件信息做匹配，把处理过的数据给用户看。

- WifiP2pManager(添加服务到本地) 	
      wifiP2pManager = (WifiP2pManager) getApplicationContext().getSystemService(WIFI_P2P_SERVICE);
- WifiP2pServiceInfo (创建本地服务的参数配置信息)
		WifiP2pDnsSdServiceInfo.newInstance("instanceName","serviceType",map);
      serviceType与NSD中的服务类型相同;
      服务信息是通过存放在map方式存入，他人连接能够获取到该数据;




