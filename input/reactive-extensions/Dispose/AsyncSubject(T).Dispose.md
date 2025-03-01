title: AsyncSubject<T>.Dispose()
---
# AsyncSubject\<T\>.Dispose Method

Unsubscribe all observers and release resources.

**Namespace:**  [System.Reactive.Subjects](System.Reactive.Subjects/System.Reactive.Subjects)  
**Assembly:**  System.Reactive (in System.Reactive.dll)

## Syntax

```vb
'Declaration
Public Sub Dispose
```

```vb
'Usage
Dim instance As AsyncSubject

instance.Dispose()
```

```csharp
public void Dispose()
```

```c++
public:
virtual void Dispose() sealed
```

```fsharp
abstract Dispose : unit -> unit 
override Dispose : unit -> unit 
```

```javascript
public final function Dispose()
```

#### Implements

[IDisposable.Dispose()](https://msdn.microsoft.com/en-us/library/es4s3w1d)

## Examples

In this example an AsyncSubject is used to subscribe to an integer sequence generated with the Range operator. An AsyncSubject only returns a value when the sequence it is subscribed to completes. Once the sequence has completed, the AsyncSubject will publish the final item in the sequence. The AsyncSubject caches the final item. Any new subscriptions against that AsyncSubject will also have the final item published to that subscription as well.

    using System;
    using System.Reactive.Linq;
    using System.Reactive.Subjects;
    using System.Reactive.Concurrency;
    using System.Threading;
    
    namespace Example
    {
      class Program
      {
        static void Main()
        {
          //*******************************************************************************************************//
          //*** A subject acts similar to a proxy in that it acts as both a subscriber and a publisher          ***//
          //*** It's IObserver interface can be used to subscribe to multiple streams or sequences of data.     ***//
          //*** The data is then published through it's IObservable interface.                                  ***//
          //***                                                                                                 ***//
          //*** In this example an AsyncSubject is used to subscribe to an integer sequence from the Range      ***//
          //*** operator. An AsyncSubject only returns a value when the sequence it is subscribed to completes. ***//
          //*** Once the sequence has completed, the AsyncSubject will publish the final item in the sequence.  ***//
          //*** The AsyncSubject caches the final item. Any new subscriptions against that AsyncSubject will    ***//
          //*** also have the final item published to that subscription as well.                                ***//
          //*******************************************************************************************************//
    
          var intSequence = Observable.Range(0, 10, Scheduler.ThreadPool);
    
          AsyncSubject<int> myAsyncSubject = new AsyncSubject<int>();
          intSequence.Subscribe(myAsyncSubject);
    
          Thread.Sleep(1000);
          myAsyncSubject.Subscribe(i => Console.WriteLine("Final integer for subscription #1 is {0}\n", i),
                                   () => Console.WriteLine("subscription #1 completed.\n"));
                                    
          
          Console.WriteLine("Sleeping for 5 seconds before subscription2\n");
          Thread.Sleep(5000);
    
          myAsyncSubject.Subscribe(i => Console.WriteLine("Final integer for subscription #2 after 5 seconds is {0}\n", i),
                                   () => Console.WriteLine("subscription #2 completed.\n"));
    
    
          Console.WriteLine("Press ENTER to exit...");
          Console.ReadLine();
    
          myAsyncSubject.Dispose();
        }
      }
    }

The following output was generated by the example code.

    Final integer for subscription #1 is 9
    
    subscription #1 completed.
    
    Sleeping for 5 seconds before subscription2
    
    Final integer for subscription #2 after 5 seconds is 9
    
    subscription #2 completed.
    
    Press ENTER to exit...

## See Also

#### Reference

[AsyncSubject\<T\> Class](AsyncSubject/AsyncSubject(T))

[System.Reactive.Subjects Namespace](System.Reactive.Subjects/System.Reactive.Subjects)






