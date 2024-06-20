
# Mono AOT module loading race condition


## iOS

Build and run the sample app, then click the button

``` console
$ dotnet build -f net8.0-ios -c Release -t:Run
```

Expected result: the button label says "4 successes, AOT"

Actual result: app crashes, the log contains messages like:

``` console
Unhandled Exception:
System.Reflection.TargetInvocationException: Arg_TargetInvocationException
 ---> System.ExecutionEngineException: Attempting to JIT compile method 'bool RacingLib.Class1:Method ()' while running in aot-only mode. See https://docs.microsoft.com/xamarin/ios/internals/limitations for more information.

   at System.Reflection.MethodBaseInvoker.InterpretedInvoke_Method(Object obj, IntPtr* args)
   at System.Reflection.MethodBaseInvoker.InvokeWithNoArgs(Object obj, BindingFlags invokeAttr)
   Exception_EndOfInnerExceptionStack
   at System.Reflection.MethodBaseInvoker.InvokeWithNoArgs(Object obj, BindingFlags invokeAttr)
   at System.Reflection.RuntimeMethodInfo.Invoke(Object obj, BindingFlags invokeAttr, Binder binder, Object[] parameters, CultureInfo culture)
   at System.Reflection.MethodBase.Invoke(Object obj, Object[] parameters)
   at MauiHello.MainPage.<>c__DisplayClass3_0.<RaceThreads>b__0()
   at System.Threading.Thread.StartCallback()
```

## Android

Build and run the sample app, then click the button

``` console
$ dotnet build -f net8.0-android -c Release -t:Run
```

Expected result: the button label says "4 successes, JIT"

Actual result: app crashes, the `adb logcat` output contains messages like:

``` console
06-20 16:48:12.649  9925 10026 F mono-rt : [ERROR] FATAL UNHANDLED EXCEPTION: System.Reflection.TargetInvocationException: Arg_TargetInvocationException
06-20 16:48:12.649  9925 10026 F mono-rt :  ---> System.InvalidProgramException: Invalid IL code in RacingLib.Class3:Method (): method body is empty.
06-20 16:48:12.649  9925 10026 F mono-rt :
06-20 16:48:12.649  9925 10026 F mono-rt :    at System.Reflection.MethodBaseInvoker.InterpretedInvoke_Method(Object obj, IntPtr* args)
06-20 16:48:12.649  9925 10026 F mono-rt :    at System.Reflection.MethodBaseInvoker.InvokeWithNoArgs(Object , BindingFlags )
06-20 16:48:12.649  9925 10026 F mono-rt :    Exception_EndOfInnerExceptionStack
06-20 16:48:12.649  9925 10026 F mono-rt :    at System.Reflection.MethodBaseInvoker.InvokeWithNoArgs(Object , BindingFlags )
06-20 16:48:12.649  9925 10026 F mono-rt :    at System.Reflection.RuntimeMethodInfo.Invoke(Object , BindingFlags , Binder , Object[] , CultureInfo )
06-20 16:48:12.649  9925 10026 F mono-rt :    at System.Reflection.MethodBase.Invoke(Object , Object[] )
06-20 16:48:12.649  9925 10026 F mono-rt :    at MauiHello.MainPage.<>c__DisplayClass3_0.<RaceThreads>b__2()
06-20 16:48:12.649  9925 10026 F mono-rt :    at System.Threading.Thread.StartCallback()
```
