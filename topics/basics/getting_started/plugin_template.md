[//]: # (title: ReSharper Platform Plugin Template)

<!-- Copyright 2000-2022 JetBrains s.r.o. and other contributors. Use of this source code is governed by the Apache 2.0 license that can be found in the LICENSE file. -->

Creating new ReSharper and Rider plugin projects is performed using the dedicated `dotnet new` template.
The generator creates all the necessary project files based on a few template inputs.

<procedure title="Create ReSharper &amp; Rider Plugins" id="create-ide-plugin">

Launch the <control>New Project</control> wizard via the <menupath>File | New | Project...</menupath> action and provide the following information:
1. Download the [plugin template]() from the GitHub Release section.
2. Install the plugin template by calling:
```
dotnet new --install JetBrains.ReSharper.SamplePlugin.*.nupkg
```
3. Unpack the template with your preferred plugin name:
```
dotnet new resharper-rider-plugin --name MyPlugin
```
> If your plugin should only target ReSharper, you can pass the `--resharper-only` switch.
>
{type="note"}

> If you plan to use GitHub Actions as your continuous integration/deployment (CI/CD) environment, you can pass the `--github-actions` switch.
>
{type="note"}

</procedure>

### Components of the Template-Generated Plugin

When extracting the template using `MyPlugin` as a name, it will create the following directory content:

```text
MyPlugin
├── .github
│   └── wrapper
│       ├── CI.yml
│       └── Deploy.yml
├── .run
│   ├── rdgen (Unix).run.xml
│   ├── rdgen (Windows).run.xml
│   ├── rdgen.run.xml
│   ├── Rider (Unix).run.xml
│   ├── Rider (Windows).run.xml
│   ├── Rider - Frontend (Unix).run.xml
│   ├── Rider - Frontend (Windows).run.xml
│   ├── Rider.run.xml
│   └── VisualStudio.run.xml
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── protocol
│   ├── src/main/kotlin/model/rider
│   └── build.gradle
├── src
│   ├── dotnet
│   │   ├── ReSharperPlugin.MyPlugin
│   │   │   ├── IMyPluginZone.cs
│   │   │   ├── ReSharperPlugin.MyPlugin.csproj
│   │   │   └── ReSharperPlugin.MyPlugin.Rider.csproj
│   │   ├── ReSharperPlugin.MyPlugin.Tests
│   │   │   ├── ReSharperPlugin.MyPlugin.Tests.csproj
│   │   │   ├── TestEnvironment.cs
│   │   │   └── test
│   │   │       └── data
│   │   │           └── nuget.config
│   │   ├── Directory.Build.props
│   │   └── Plugin.props
│   └── rider/main
│       ├── kotlin/com/jetbrains/rider/plugins/myplugin
│       └── resources
│           └── META-INF
│               └── plugin.xml
├── tools
│   ├── nuget.exe
│   └── vswhere.exe
├── .gitattributes
├── .gitignore
├── CHANGELOG.md
├── README.md
├── ReSharperPlugin.MyPlugin.sln
├── build.gradle
├── buildPlugin.ps1
├── gradle.properties
├── gradlew
├── gradlew.bat
├── publishPlugin.ps1
├── runVisualStudio.ps1
├── settings.gradle
└── settings.ps1
```

* The <path>gradlew</path> and <path>gradlew.bat</path> files to bootstrap running Gradle on Windows, macOS, and Linux. These will also automatically install the required [Amazon Corretto 11 SDK](https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/downloads-list.html) into the <path>build/gradle-jvm</path> directory.
* The <path>build.gradle</path> file to build and deploy the plugin for ReSharper and Rider.
> See the IntelliJ SDK help for more information about [Gradle-Related Plugin Components](https://plugins.jetbrains.com/docs/intellij/gradle-prerequisites.html#components-of-a-wizard-generated-gradle-intellij-platform-plugin).
* The <path>buildPlugin.ps1</path> and <path>publishPlugin.ps1</path> files to build and deploy only the ReSharper plugin.
* The <path>gradle.properties</path>, <path>settings.ps1</path>, <path>plugin.xml</path>, and <path>Plugin.props</path> files, containing information about plugin metadata (id, description, authors), dependency versions (ReSharper/ Rider SDK), and paths to solution and project files.
* The [Run/debug configurations](https://www.jetbrains.com/help/rider/Run_Debug_Configuration.html) for Rider/IntelliJ IDEA (<path>.run</path> directory) and the <path>runVisualStudio.ps1</path> for PowerShell (`--resharper-only` template extraction) to launch the plugin in an experimental instance of Rider or Visual Studio with ReSharper.
* The <path>src/dotnet</path> directory with basic project files for ReSharper and Rider including a test project. The <path>Directory.Build.props</path> defines all common NuGet references for both.
* The <path>README.md</path> and <path>CHANGELOG.md</path> files for documentation about the plugin. The first section of the <path>CHANGELOG.md</path> is automatically included in the plugin metadata. The <path>README.md</path> has predefined [Shields.IO](https://shields.io/) badges with `RIDER_PLUGIN_ID` and `RESHARPER_PLUGIN_ID` as placeholders that can be updated after the first deployment.
* The <path>.github/workflow</path> directory (requires `--github-actions` during template extraction) with predefined workflows for a continuous build (including `Build` and `Test`) and a deployment build.
