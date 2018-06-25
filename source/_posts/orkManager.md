title: WorkManager
author: 大帅
date: 2018-05-29 16:02:12
tags:
---
### [WorkManager](https://codelabs.developers.google.com/codelabs/android-workmanager/)
>    主要的类
	
    WorkManager：管理任务，开启任务

1. WorkManager
	- 方法
    	  WorkManager getInstance()：
          获取实例
          initialize(@NonNull Context context, @NonNull Configuration configuration)：
          获取实例，参数二配置线程池
          enqueue(@NonNull WorkRequest... workRequest)：
          添加后台任务
          enqueue(@NonNull List<? extends WorkRequest> baseWork)：
          添加后台任务
          WorkContinuation beginWith(@NonNull OneTimeWorkRequest...work)：
          开启不需要重复的任务，它只执行一遍，返回值WorkContinuation 是用来开启任务链的；
          能够与其他的进行then、combine操作
          WorkContinuation beginWith(@NonNull List<OneTimeWorkRequest> work)；
  		开启一组的上述任务
 		 WorkContinuation beginUniqueWork(
            @NonNull String uniqueWorkName,
            @NonNull ExistingWorkPolicy existingWorkPolicy,
            @NonNull OneTimeWorkRequest... work) ：
  		该方法第一个参数是任务名称，名称是唯一的不能够重复；第二个参数ExistingWorkPOneTimeWorkRequestolicy是用来判断如果出现了重复的，是替换还是追加、还是不理会
 		 继续保持当前的任务目标不变；
          cancelWorkById(@NonNull UUID id)：
  		根据uuid去取消任务，但是方法的调用不一定回起作用，如果正在执行，那么将不会取消，任务会正常的去完成。
  		void cancelAllWorkByTag(@NonNull String tag)：
  		与上相似。
  		void cancelUniqueWork(@NonNull String uniqueWorkName)
  		字面意思。
  		LiveData<WorkStatus> getStatusById(@NonNull UUID id);
  		能够监听任务的完成情况。
  		LiveData<List<WorkStatus>> getStatusesByTag(@NonNull String tag)：
  		字面。
  		LiveData<List<WorkStatus>> getStatusesForUniqueWork(
            @NonNull String uniqueWorkName)：
  		字面。
  		SynchronousWorkManager synchronous()：
  		返回的对象方法什么的跟原来的WorkManager方法都差不多，这个是同步方法，不能在主线程中运行。在子线程中使用，能直接知道是否添加成功
2. WorkRequest
  > 任务请求
  
  - OneTimeWorkRequest 
  		不重复的请求，执行一次；
  - PeriodicWorkRequest
  		重复执行
3. WorkSpec
  	 存入数据库的一些任务调度信息，时间、任务信息等
4. Worker
  > 任务基本单元，有已经实现好的几个类，自己实现只需要复写 dowork就可以，类单独写，不能是内部类实现，否则实例化不了。通过get方法能获取到传递的所有信息数据。下列出已经实现的该类的对象。
  
  - CombineContinuationsWorker
  		工作任务流组合，能够像链条一样
  - ConstraintTrackingWorker
  		当约束符合条件,可以传如参数（类名称），通过不同参数实现不同代理
5. Constraints
 > 约束，设定任务在什么系统的什么情况下能够启动

	- mRequiredNetworkType
    	  什么网络条件下启动
    - mRequiresCharging 
    		充电
    - mRequiresDeviceIdle
    		休眠
    - mRequiresBatteryNotLow
    		电量不高
    - mRequiresStorageNotLow
    		内存不高
    - mContentUriTriggers
    		uri触发启动
 
        
        
        