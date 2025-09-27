# hamarb123.ProjectSdks.ILSingleFile
A .NET Project SDK to enable running IL files with `dotnet run app.cs` (.NET 10 feature).

NuGet link:
[![NuGet version (hamarb123.ProjectSdks.ILSingleFile)](https://img.shields.io/nuget/v/hamarb123.ProjectSdks.ILSingleFile.svg?style=flat-square)](https://www.nuget.org/packages/hamarb123.ProjectSdks.ILSingleFile/)

To make it work, you need to write your C# file like so - the project processor uses pattern matching to find the multi-line raw string literal:
```csharp
// Fibonacci.cs:
// Example usage (when marked executable on unix): ./Fibonacci.cs 20 --> 6765

#!/usr/bin/dotnet run
#:sdk hamarb123.ProjectSdks.ILSingleFile@10.0.0-rc.1

_ = """
.assembly extern System.Runtime { }
.assembly extern System.Console { }
.assembly Fibonacci { }
.module Fibonacci.exe

.class private auto ansi beforefieldinit Program
	extends [System.Runtime]System.Object
{
	.method private hidebysig static void Main(string[] 'args') cil managed
	{
		// Fibonacci

		.entrypoint
		.maxstack 3
		.locals (int32 'remaining', int32 'a', int32 'b')

		ldarg.0
		ldc.i4.0
		ldelem.ref
		call int32 [System.Runtime]System.Int32::Parse(string)
		stloc 'remaining'
		ldc.i4.0
		stloc 'a'
		ldc.i4.1
		stloc 'b'

		again:
		ldloc 'remaining'
		ldc.i4.0
		ceq
		brtrue 'done'

		ldloc 'b'
		ldloc 'a'
		ldloc 'b'
		add
		stloc 'b'
		stloc 'a'

		ldloc 'remaining'
		ldc.i4.1
		sub
		stloc 'remaining'
		br 'again'

		done:
		ldloc 'a'
		call void [System.Console]System.Console::WriteLine(int32)
		ret
	}
}
""";
```

The following custom properties are available:
- `EnableFold`: Enable IL method body folding (true by default)
- `EnableOptimize`: Enable IL opcode optimization (true by default)
- `ExtraIlasmFlags`: Allows additional ilasm flags

Working package versions:
- `10.0.0-rc.1` (.NET 10.0 RC1)
