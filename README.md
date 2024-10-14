# Inclure Small_IMGUI dans un projet visual studio c#

## 1. Récupérer le code de Small_IMGUI

### Option "projet à l'arrache"

cloner le repo Small_IMGUI directement dans le dossier de votre projet :
```sh
cd c:\mon_projet
mkdir deps
cd deps
git clone -b small https://github.com/small-studio/Small_IMGUI
```

### Option "projet clean"
ajouter le repo Small_IMGUI en submodule. Cela permet de se mettre à jour si nécessaire, de pusher des modifs sur Small_IMGUI directement depuis le projet, etc.
```sh
cd c:\mon_projet
git submodule add -b small https://github.com/small-studio/Small_IMGUI deps/Small_IMGUI
```

## 2. Inclure le `.csproj` Small_IMGUI dans votre solution Visual

- Dans votre solution visual Faire `Fichier/Ajouter/Projet Existant`
- Selectionner le fichier .csproj de Small_IMGUI que vous avez cloné ou ajouté en submodule. Il doit se trouver ici `mon_projet\deps\Small_IMGUI\src\Small_IMGUI\Small_IMGUI.csproj`
- une nouveau projet intitulé `Small_IMGUI` devrait maintenant apparaître dans votre solution
- faire clic droit dessus, puis `Ouvrir dans Terminal`
- exécuter la commande suivante : `dotnet build`
- Ajouter Small_IMGUI en dépendance du projet où vous souhaitez l'utiliser :
    - Clic droit sur votre projet cible, puis `Dépendences de build/Dépendances du projet`
        - cocher Small_IMGUI
        - cliquer sur OK
    - Clic droit sur votre projet cible, puis `Ajouter/Référence de projet...`
        - cocher Small_IMGUI
        - cliquer sur OK

Vous pouvez maintenant instantier dans votre code une nouvelle fenêtre Small_IMGUI. Pour ce faire :
- Créer une classe qui dérive de `Small_IMGUI.Window`
- overrider la fonction `SubmitUI`
- L'instantier dans votre fonction `Main`, ou tout autre fonction en fonction de vos besoins
- **Attention :** la fonction `Run()` est bloquante tant que la fenêtre n'est pas fermée

```c#
public class MyWindow : Small_IMGUI.Window
{
    public override void SubmitUI()
    {
        // écrivez le code Imgui que vous souhaitez ici !!!
        ImGui.Text("Hello, world!");

        // vous pouvez vous inspirer du code a l'interieur de la fonction suivante : 
        // base.SubmitUI();
    }
}

internal class Program
{
    static void Main(string[] args)
    {
        MyWindow window = new MyWindow();
        window.Run();
    }
}
```

# ImGui.NET

This is a .NET wrapper for the immediate mode GUI library, Dear ImGui (https://github.com/ocornut/imgui). ImGui.NET lets you build graphical interfaces using a simple immediate-mode style. ImGui.NET is a .NET Standard library, and can be used on all major .NET runtimes and operating systems.

Included is a basic sample program that shows how to use the library, and renders the UI using [Veldrid](https://github.com/veldrid/veldrid), a portable graphics library for .NET. By itself, Dear ImGui does not care what technology you use for rendering; it simply outputs textured triangles. Example renderers also exist for MonoGame and OpenTK (OpenGL).

This wrapper is built on top of [cimgui](https://github.com/cimgui/cimgui), which exposes a plain C API for Dear ImGui. If you are using Windows, OSX, or a mainline Linux distribution, then the ImGui.NET NuGet package comes bundled with a pre-built native library. If you are using another operating system, then you may need to build the native library yourself; see the cimgui repo for build instructions.

[![NuGet](https://img.shields.io/nuget/v/ImGui.NET.svg)](https://www.nuget.org/packages/ImGui.NET)

___As of February 2023, I (@mellinoe) am no longer able to publicly share updates to ImGui.NET and related libraries. A big thanks to @zaafar who continues to actively maintain the library and keep it up to date with new versions of native Dear ImGui. Feel free to join the [Discord server](https://discord.gg/s5EvvWJ) for more information about the current status of development.___

# Building

ImGui.NET can be built in Visual Studio or on the command line. The .NET Core SDK is needed to build on the command line, and it can be downloaded [here](https://www.microsoft.com/net/core). Visual Studio 2017 is the minimum VS version supported for building.

# Usage

ImGui.NET currently provides a raw wrapper around the ImGui native API, and also provides a very thin safe, managed API for convenience. It is currently very much like using the native library, which is very simple, flexible, and robust. The easiest way to figure out how to use the library is to read the documentation of imgui itself, mostly in the imgui.cpp, and imgui.h files, as well as the exported functions in cimgui.h. Looking at the [sample program code](https://github.com/ImGuiNET/ImGui.NET/tree/master/src) will also give some indication about basic usage.

# Debugging native code

ImGui.NET is a wrapper over native code. By default, this native code is packaged and released in an optimized form, making debugging difficult. To obtain a debuggable version of the native code, follow these steps:

1. Clone the [ImGui.NET-nativebuild](https://github.com/ImGuiNET/ImGui.NET-nativebuild) repo, at the tag matching the version of ImGui.NET you are using.
2. In the ImGui.NET-nativebuild repo, run `build.cmd debug` or `build.sh debug` (depending on your platform).
3. Copy the produced binaries (cimgui.dll, libcimgui.so, or libcimgui.dylib) into your application.
4. Run the program under a native debugger, or enable mixed-mode debugging in Visual Studio.

# See Also

https://github.com/ocornut/imgui
> Dear ImGui is a bloat-free graphical user interface library for C++. It outputs optimized vertex buffers that you can render anytime in your 3D-pipeline enabled application. It is fast, portable, renderer agnostic and self-contained (no external dependencies).

> Dear ImGui is designed to enable fast iterations and to empower programmers to create content creation tools and visualization / debug tools (as opposed to UI for the average end-user). It favors simplicity and productivity toward this goal, and lacks certain features normally found in more high-level libraries.

> Dear ImGui is particularly suited to integration in games engine (for tooling), real-time 3D applications, fullscreen applications, embedded applications, or any applications on consoles platforms where operating system features are non-standard.

See the [official screenshot thread](https://github.com/ocornut/imgui/issues/123) for examples of many different kinds of interfaces created with Dear ImGui.

https://github.com/cimgui/cimgui
> This is a thin c-api wrapper for the excellent C++ intermediate gui imgui. This library is intended as a intermediate layer to be able to use imgui from other languages that can interface with C .
