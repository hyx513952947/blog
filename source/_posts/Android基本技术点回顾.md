title: Android 基本技术点无线设备
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
#### WiFi p2p连接
> p2p不需要访问网络，但是会使用java中的socket，socket需网络权限(  CHANGE_WIFI_STATE、ACCESS_WIFI_STATE、INTERNET)

- 设置广播接收器和p2p管理器
	首先注册广播，监听每次的wifi p2p事件变化，主要监听`WIFI_P2P_STATE_CHANGED_ACTION Wi-Fi P2P　是否开启`，`WIFI_P2P_PEERS_CHANGED_ACTION 对等节点（peer）列表发生了变化`,`WIFI_P2P_CONNECTION_CHANGED_ACTION 连接状态发生了改变`,`WIFI_P2P_THIS_DEVICE_CHANGED_ACTION 设备的详细配置发生了变化`这些广播，这些广播定义在`WifiP2pManager`类中。
    实例化`Channel`对象，在后续的方法中基本都需要用到，`WifiP2pManager`的方法基本都需要这个，应该是传递数据封装数据用的。
    
      mChannel = mManager.initialize(this, getMainLooper(), null);
        
- 初始化对等节点发现（Peer Discovery）
 	
    discoverPeers() 搜索设备

- 基本概念
	  WPS是啥？？？？：
      （Wi-Fi Protected Setup，Wi-Fi保护设置）（有的叫做AOSS、有的叫做QSS，不过功能都一致。）是由Wi-Fi联盟组织实施的认证项目，主要致力于简化无线局域网的安装及安全性能配置工作。在传统方式下，用户新建一个无线网络时，必须在接入点手动设置网络名（SSID）和安全密钥，然后在客户端验证密钥以阻止“不速之客”的闯入。这整个过程需要用户具备Wi-Fi设备的背景知识和修改必要配置的能力。Wi- Fi Protected Setup能帮助用户自动设置网络名（SSID）、配置强大的WPA数据编码及认证功能，用户只需输入个人信息码（PIN方法）或按下按钮（按钮设置，或称PBC），即能安全地连入WLAN。这大大简化了无线安全设置的操作。Wi-Fi Protected Setup支持多种通过Wi-Fi认证的802.11产品，包括接入点、无线适配器、Wi-Fi电话以及其他消费性电子设备。
      WPS分为PBC(BUTTON)和PIN两种方式：
      A PBC: 按WPS按钮实现WPS安全连接.
		在AP中，在WPS设置中,设置为启用.
		按一下客户端(无线网卡)上的WPS按键,搜索WPS网络.
		按一下AP上的WPS按键,WPS开始链接协商,片刻后WPS安全连接成功建立.
	  B PIN
	    B1) PIN(Internal Registra, 相对于AP而言)：通过在路由器中输入客户端PIN码来实现WPS安全连接.
		 在WPS设置中,把状态设置为启用.
     	打开客户端WPS设置软件,选择在路由器中输入PIN的方式连接,同时软件上还会显示客户端当前的PIN码.
    	 打开路由器界面,在WPS模式里选择PIN模式,然后输入客户端的PIN码,点添加新设备,一会儿后,WPS安全连接成功建立.
 		B2) PIN(External Registra, 相对于AP而言)：通过输入AP的PIN码实现WPS安全连接.在AP中，在WPS设置中,设置为启用.  记住AP的PIN码,然后打开客户端（无线网卡）WPS设置软件,选择以AP的PIN码来进行连接.
        输入完PIN码后,点下一步,一会儿后,WPS安全连接成功建立.

- 代码


	public class WifiP2PActivity extends AppCompatActivity {
    String TAG = " WifiP2P";
    WifiP2pManager mWifiP2pManager;
    Channel mChannel;
    IntentFilter mIntentFilter;
    WifiP2PBReciver mReciver;
    List<WifiP2pDevice> peers = new ArrayList();
    WifiP2pConfig mWifiP2pConfig;
    //获取设备列表
    private WifiP2pManager.PeerListListener peerListListener = new WifiP2pManager.PeerListListener() {
        @Override
        public void onPeersAvailable(WifiP2pDeviceList peerList) {
            // 获取设备列表 peerList.getDeviceList();
            Log.i(TAG,"获取的对等节点列表："+peerList.toString());
            peers.clear();
            peers.addAll(peerList.getDeviceList());
            // 连接 不应该在这写，这是动态变化的，应该手动选择时从保存的对等节点列表中获取信息，去连接
            mWifiP2pConfig = new WifiP2pConfig();
            mWifiP2pConfig.deviceAddress = peers.get(0).deviceAddress;
            mWifiP2pConfig.wps.setup = WpsInfo.PBC;
            mWifiP2pManager.connect(mChannel, mWifiP2pConfig, new WifiP2pManager.ActionListener() {
                @Override
                public void onSuccess() {
                    Log.i(TAG,"连接成功了");
                }

                @Override
                public void onFailure(int reason) {
                    Log.i(TAG,"连接失败了:"+reason);
                }
            });
        }
    };
    WifiP2pManager.ConnectionInfoListener connectionInfoListener = new WifiP2pManager.ConnectionInfoListener() {
        @Override
        public void onConnectionInfoAvailable(WifiP2pInfo info) {
            // InetAddress from WifiP2pInfo struct.
            //当有多个设备同时试图连接到一台设备时（例如多人游戏或者聊天群），这一台设备将被指定为“群主”（group owner）。
            String groupOwnerAddress = info.groupOwnerAddress.getHostAddress();
            // After the group negotiation, we can determine the group owner.
            if (info.groupFormed && info.isGroupOwner) {
                // Do whatever tasks are specific to the group owner.
                // One common case is creating a server thread and accepting
                // incoming connections.
            } else if (info.groupFormed) {
                // The other device acts as the client. In this case,
                // you'll want to create a client thread that connects to the group
                // owner.
            }
        }
    };
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_wifi_p2_p);

        mIntentFilter = new IntentFilter();
        //连接状态发生了改变
        mIntentFilter.addAction(WifiP2pManager.WIFI_P2P_CONNECTION_CHANGED_ACTION);
        //　指示设备的详细配置发生了变化
        mIntentFilter.addAction(WifiP2pManager.WIFI_P2P_THIS_DEVICE_CHANGED_ACTION);
        //代表对等节点（peer）列表发生了变化
        mIntentFilter.addAction(WifiP2pManager.WIFI_P2P_PEERS_CHANGED_ACTION);
        //　指示　Wi-Fi P2P　是否开启
        mIntentFilter.addAction(WifiP2pManager.WIFI_P2P_STATE_CHANGED_ACTION);

        mWifiP2pManager = (WifiP2pManager) getApplicationContext().getSystemService(WIFI_P2P_SERVICE);

        mChannel = mWifiP2pManager.initialize(this, getMainLooper(), new WifiP2pManager.ChannelListener() {
            @Override
            public void onChannelDisconnected() {
                Log.i(TAG,"通道断开");
            }
        });
        //搜索对等节点 peers
        mWifiP2pManager.discoverPeers(mChannel, new WifiP2pManager.ActionListener() {
            @Override
            public void onSuccess() {
                Log.i(TAG,"搜索成功！");
            }

            @Override
            public void onFailure(int reason) {
                //WifiP2pManager 中的失败值
                /*** Passed with {@link WifiP2pManager.ActionListener#onFailure}.
                 * Indicates that the operation failed due to an internal error.
                */int ERROR               = 0;

                /**
                 * Passed with {@link WifiP2pManager.ActionListener#onFailure}.
                 * Indicates that the operation failed because p2p is unsupported on the device.
                 */int P2P_UNSUPPORTED     = 1;
                /**
                 * Passed with {@link WifiP2pManager.ActionListener#onFailure}.
                 * Indicates that the operation failed because the framework is busy and
                 * unable to service the request
                 */int BUSY = 2;
                /**
                 * Passed with {@link WifiP2pManager.ActionListener#onFailure}.
                 * Indicates that the {@link #discoverServices} failed because no service
                 * requests are added. Use {@link #addServiceRequest} to add a service
                 * request.
                 */int NO_SERVICE_REQUESTS = 3;
                Log.e(TAG,"搜索失败！"+reason);
            }
        });
    }

    @Override
    protected void onResume() {
        super.onResume();
        //注册接受wifi p2p 状态变化广播
        mReciver = new WifiP2PBReciver();
        getApplicationContext().registerReceiver(mReciver,mIntentFilter);
    }

    @Override
    protected void onPause() {
        super.onPause();
        try{
            unregisterReceiver(mReciver);
        }catch (Exception e){

        }
    }

    class WifiP2PBReciver extends BroadcastReceiver{

        @Override
        public void onReceive(Context context, Intent intent) {
            String action = intent.getAction();
            if (WifiP2pManager.WIFI_P2P_STATE_CHANGED_ACTION.equals(action)) {
                // Determine if Wifi P2P mode is enabled or not, alert
                // the Activity.
                int state = intent.getIntExtra(WifiP2pManager.EXTRA_WIFI_STATE, -1);

                if (state == WifiP2pManager.WIFI_P2P_STATE_ENABLED) {

                } else {

                }
            } else if (WifiP2pManager.WIFI_P2P_PEERS_CHANGED_ACTION.equals(action)) {

                // The peer list has changed!  We should probably do something about
                // that.
               //在此处获取设备列表变化后的数据
                mWifiP2pManager.requestPeers(mChannel,peerListListener);

            } else if (WifiP2pManager.WIFI_P2P_CONNECTION_CHANGED_ACTION.equals(action)) {

                // Connection state changed!  We should probably do something about
                // that.
                NetworkInfo networkInfo = (NetworkInfo) intent
                        .getParcelableExtra(WifiP2pManager.EXTRA_NETWORK_INFO);

                if (networkInfo.isConnected()) {

                    // We are connected with the other device, request connection
                    // info to find group owner IP

                    mWifiP2pManager.requestConnectionInfo(mChannel, connectionInfoListener);
                }

            } else if (WifiP2pManager.WIFI_P2P_THIS_DEVICE_CHANGED_ACTION.equals(action)) {
                WifiP2pDevice device = (WifiP2pDevice) intent.getParcelableExtra(WifiP2pManager.EXTRA_WIFI_P2P_DEVICE);

            }
        }
    }
    }
---- 
### WiFi P2P 服务发现

   - 首先在本地注册一个服务，服务注册是要将服务类型、服务信息封装到WifiP2pServiceInfo中，再通过WifiP2pManager将服务添加到本地，本地服务添加后注册监听DnsSdTxtRecordListener来实时监听record记录数据，注册DnsSdServiceResponseListener接收服务的实际描述和连接的信息，前者处理记录连接的服务信息用来跟后者做对应关系处理。前者给你一组对他的服务的描述信息，然后在后者进行匹配，将描述信息与实际的硬件信息做匹配，把处理过的数据给用户看。连接后创建服务请求 addServiceRequest()，最后调用 discoverServices()


- WifiP2pManager(添加服务到本地) 	
      wifiP2pManager = (WifiP2pManager) getApplicationContext().getSystemService(WIFI_P2P_SERVICE);
- WifiP2pServiceInfo (创建本地服务的参数配置信息)
		WifiP2pDnsSdServiceInfo.newInstance("instanceName","serviceType",map);
      serviceType与NSD中的服务类型相同;
      服务信息是通过存放在map方式存入，他人连接能够获取到该数据;
 