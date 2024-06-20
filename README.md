
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

