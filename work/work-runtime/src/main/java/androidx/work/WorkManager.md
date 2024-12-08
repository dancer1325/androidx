* `WorkManager`
  * 👀== recommended library -- for -- persistent work👀
  * allows
    * observation of work status
    * ability to create complex chains of work == enqueue requests

    ```
    WorkManager workManager = WorkManager.getInstance(Context);
    workManager.enqueue(new OneTimeWorkRequest.Builder(FooWorker.class).build());
    ```

  * if it's available -> uses an underlying job / dispatch service -- based on the -- criteria
    * if API 23+ -> uses JobScheduler 
    * if API 14-22 -> uses a custom `AlarmManager` + `BroadcastReceiver` implementation
  * if its `Constraints` are met -> guaranteed to execute sometime AFTER
  * ALL work -- must be -- done | `ListenableWorker` class
  * background work's maximum time to finish this execution =  10'
    * AFTER this time -> the worker -- will be signalled to -- stop
  * types of work supported
    * `OneTimeWorkRequest`
    * `PeriodicWorkRequest`
  * auto-initializes itself
    * by default, -- via -- built-in `ContentProvider`
      * suitable for MOST developers
    * you can provide a custom `Configuration` -- via -- `Configuration.Provider` or `WorkManager.initialize`

* `ContentProviders`
  * are created and run BEFORE the `Application` object
    * -> WorkManager singleton can be setup BEFORE your code can run

* `Worker`
  * == simple implementation of `WorkManager`
  * recommendations
    * starting point for most developers

* `CoroutineWorker` or `RxWorker`
  * OTHER implementations

* `WorkRequest`
  * has an associated id
    * uses
      * lookups
      * observation

    ```
    WorkRequest request = new OneTimeWorkRequest.Builder(FooWorker.class).build();
    workManager.enqueue(request);
    LiveData<WorkInfo> status = workManager.getWorkInfoByIdLiveData(request.getId());
    status.observe(...);
    
    // 2. For cancellation
    WorkRequest request = new OneTimeWorkRequest.Builder(FooWorker.class).build();
    workManager.enqueue(request);
    workManager.cancelWorkById(request.getId());
    
    // 3. Alternative to chain work
    WorkRequest request1 = new OneTimeWorkRequest.Builder(FooWorker.class).build();
    WorkRequest request2 = new OneTimeWorkRequest.Builder(BarWorker.class).build();
    WorkRequest request3 = new OneTimeWorkRequest.Builder(BazWorker.class).build();
    workManager.beginWith(request1, request2).then(request3).enqueue();
    ```
  * can
    * accept
      * `Constraints`,
      * inputs (see `Data`),
      * backoff criteria
    * be tagged with
      * human-readable Strings (see `WorkRequest.Builder.addTag`),
      * chains of work
    * -- be given a -- uniquely-identifiable name (see `beginUniqueWork`)
  * _Example:_ 
    ```
                 A
                 |
        +----------+
        |          |
        B          C
        |
        +----+
        |    |
        D    E
    
    WorkContinuation continuation = workManager.beginWith(A);
    continuation.then(B).then(D, E).enqueue();  // A is implicitly enqueued here
    continuation.then(C).enqueue();
    ```

* `Work`
  * if ALL its prerequisites are complete -> eligible for execution
  * if ANY of its prerequisites fail OR cancelled -> work will NEVER run

* ### Renaming and Removing ListenableWorker Classes
* TODO:
* Exercise caution in renaming classes derived from [ListenableWorker]s. WorkManager stores the
* class name in its internal database when the [WorkRequest] is enqueued so it can later create an
* instance of that worker when constraints are met. Unless otherwise specified in the WorkManager
* [Configuration], this is done in the default [WorkerFactory] which tries to reflectively create
* the ListenableWorker object. Therefore, renaming or removing these classes is dangerous - if
* there is pending work with the given class, it will fail permanently if the class cannot be
* found. If you are using a custom WorkerFactory, make sure you properly handle cases where the
* class is not found so that your code does not crash.
*
* In case it is desirable to rename a class, implement a custom WorkerFactory that instantiates the
* right ListenableWorker for the old class name.