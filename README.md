# XUnity 自动翻译器

## 目录
 * [介绍](#介绍)
 * [插件框架](#插件框架)
 * [安装](#安装)
 * [按键映射](#按键映射)
 * [翻译器](#翻译器)
 * [文本框架](#文本框架)
 * [配置](#配置)
 * [IL2CPP 支持](#il2cpp-支持)
 * [常见问题](#常见问题)
 * [翻译模组](#翻译模组)
 * [手动翻译](#手动翻译)
 * [关于重新分发](#关于重新分发)
 * [纹理翻译](#纹理翻译)
 * [与自动翻译器集成](#与自动翻译器集成)
 * [实现翻译器](#实现翻译器)
 * [实现资源重定向器](#实现资源重定向器)
 
## 介绍
这是一个高级翻译插件，可以用来自动翻译基于Unity的游戏，也提供了手动翻译游戏所需的工具。

它确实会（显然）连接到互联网，以便提供自动翻译，所以如果您对此不满意，请不要使用它。

如果您打算将此插件作为游戏翻译套件的一部分进行重新分发，请阅读[这一部分](#关于重新分发)和关于[手动翻译](#手动翻译)的部分，以便您了解插件的工作原理。

## 插件框架
该模组可以在没有任何外部依赖的情况下安装，或者作为以下插件管理器/模组加载器的插件安装：
 * [BepInEx](https://github.com/bbepis/BepInEx)（推荐）
 * [MelonLoader](https://melonwiki.xyz)
 * [IPA](https://github.com/Eusth/IPA)
 * UnityInjector

所有方法的安装说明都可以在下面找到。

## 安装
插件可以通过以下方式安装：

### 独立安装（ReiPatcher）
需要：无需其他依赖，本下载包提供了ReiPatcher。

*非常重要的说明：使用此方法是让插件在大多数Unity游戏中工作的确定方法，只需简单两次点击。请注意，如果使用了支持的插件管理器之一，则应避免使用此安装方法，因为它会导致问题。*

 0. 阅读上面的`非常重要的说明`。
 1. 从[发布页面](../../releases)下载 XUnity.AutoTranslator-ReiPatcher-{版本}.zip。
 2. 直接解压到游戏目录中，使 "SetupReiPatcherAndAutoTranslator.exe" 与其他exe文件放在一起。
 3. 执行 "SetupReiPatcherAndAutoTranslator.exe"。这将正确设置ReiPatcher。
 4. 执行在现有可执行文件旁边创建的快捷方式 {游戏名称} (Patch and Run).lnk。这将修补并启动游戏。
 5. 从现在开始，您可以从 {游戏名称}.exe 启动游戏。
 6. 由于各种考虑，默认情况下并非所有文本钩子都被启用，因此如果您发现游戏或游戏的某些部分没有被正确翻译，可能值得进入配置文件并启用一些已禁用的文本框架！配置文件在游戏启动时创建。

文件结构应该像这样：
```
{游戏目录}/ReiPatcher/Patches/XUnity.AutoTranslator.Patcher.dll
{游戏目录}/ReiPatcher/ExIni.dll
{游戏目录}/ReiPatcher/Mono.Cecil.dll
{游戏目录}/ReiPatcher/Mono.Cecil.Inject.dll
{游戏目录}/ReiPatcher/Mono.Cecil.Mdb.dll
{游戏目录}/ReiPatcher/Mono.Cecil.Pdb.dll
{游戏目录}/ReiPatcher/Mono.Cecil.Rocks.dll
{游戏目录}/ReiPatcher/ReiPatcher.exe
{游戏目录}/{游戏名称}_Data/Managed/ReiPatcher.exe
{游戏目录}/{游戏名称}_Data/Managed/XUnity.Common.dll
{游戏目录}/{游戏名称}_Data/Managed/XUnity.ResourceRedirector.dll
{游戏目录}/{游戏名称}_Data/Managed/XUnity.AutoTranslator.Plugin.Core.dll
{游戏目录}/{游戏名称}_Data/Managed/XUnity.AutoTranslator.Plugin.ExtProtocol.dll
{游戏目录}/{游戏名称}_Data/Managed/MonoMod.RuntimeDetour.dll
{游戏目录}/{游戏名称}_Data/Managed/MonoMod.Utils.dll
{游戏目录}/{游戏名称}_Data/Managed/Mono.Cecil.dll
{游戏目录}/{游戏名称}_Data/Managed/0Harmony.dll
{游戏目录}/{游戏名称}_Data/Managed/ExIni.dll
{游戏目录}/{游戏名称}_Data/Managed/Translators/{翻译器}.dll
{游戏目录}/AutoTranslator/Translation/任意翻译文件.txt（这些文件将由插件自动生成！）
 ```

**注意：** 放置在ReiPatcher目录中的 `Mono.Cecil.dll` 文件与放置在Managed目录中的文件不同。

### BepInEx 插件
需要：[BepInEx插件管理器](https://github.com/BepInEx/BepInEx)（请先按照其安装说明进行安装！）。

 1. 从[发布页面](../../releases)下载 XUnity.AutoTranslator-BepInEx-{版本}.zip。
 2. 直接解压到游戏目录中，使插件dll文件放入BepInEx文件夹。
 3. 启动游戏。
 4. 由于各种考虑，默认情况下并非所有文本钩子都被启用，因此如果您发现游戏或游戏的某些部分没有被正确翻译，可能值得进入配置文件并启用一些已禁用的文本框架！配置文件在游戏启动时创建。

文件结构应该像这样：
```
{游戏目录}/BepInEx/core/XUnity.Common.dll
{游戏目录}/BepInEx/plugins/XUnity.ResourceRedirector/XUnity.ResourceRedirector.dll
{游戏目录}/BepInEx/plugins/XUnity.ResourceRedirector/XUnity.ResourceRedirector.BepInEx.dll
{游戏目录}/BepInEx/plugins/XUnity.AutoTranslator/XUnity.AutoTranslator.Plugin.Core.dll
{游戏目录}/BepInEx/plugins/XUnity.AutoTranslator/XUnity.AutoTranslator.Plugin.BepInEx.dll
{游戏目录}/BepInEx/plugins/XUnity.AutoTranslator/XUnity.AutoTranslator.Plugin.ExtProtocol.dll
{游戏目录}/BepInEx/plugins/XUnity.AutoTranslator/ExIni.dll
{游戏目录}/BepInEx/plugins/XUnity.AutoTranslator/Translators/{翻译器}.dll
{游戏目录}/BepInEx/core/MonoMod.RuntimeDetour.dll
{游戏目录}/BepInEx/core/MonoMod.Utils.dll
{游戏目录}/BepInEx/core/Mono.Cecil.dll
{游戏目录}/BepInEx/Translation/任意翻译文件.txt（这些文件将由插件自动生成！）
```

#### BepInEx IL2CPP 插件
IL2CPP的安装说明与标准版本相同，除了您必须为IL2CPP安装BepInEx 6，截至撰写本文时，这些版本仅作为前沿构建版本[在这里](https://builds.bepis.io/projects/bepinex_be)提供，并且您必须使用此插件的`BepInEx-IL2CPP`包。

当前版本（5.4.0）是基于前沿构建版本704构建的。

### MelonLoader 插件
需要：[Melon Loader](https://melonwiki.xyz)（请先按照其安装说明进行安装！）。

 1. 从[发布页面](../../releases)下载 XUnity.AutoTranslator-MelonMod-{版本}.zip。
 2. 直接解压到游戏目录中，使插件dll文件放入Mods和UserLibs文件夹。
 3. 启动游戏。
 4. 由于各种考虑，默认情况下并非所有文本钩子都被启用，因此如果您发现游戏或游戏的某些部分没有被正确翻译，可能值得进入配置文件并启用一些已禁用的文本框架！配置文件在游戏启动时创建。

文件结构应该像这样：
```
{游戏目录}/Mods/XUnity.AutoTranslator.Plugin.MelonMod.dll
{游戏目录}/UserLibs/XUnity.Common.dll
{游戏目录}/UserLibs/XUnity.ResourceRedirector.dll
{游戏目录}/UserLibs/XUnity.AutoTranslator.Plugin.Core.dll
{游戏目录}/UserLibs/XUnity.AutoTranslator.Plugin.ExtProtocol.dll
{游戏目录}/UserLibs/ExIni.dll
{游戏目录}/UserLibs/Translators/{翻译器}.dll
{游戏目录}/AutoTranslator/Translation/任意翻译文件.txt（这些文件将由插件自动生成！）
```

当前版本（5.4.0）是基于v0.6.1 Open-Beta构建的。

#### MelonLoader IL2CPP 插件
IL2CPP的安装说明与标准版本相同，除了您必须使用此插件的`MelonMod-IL2CPP`包。

### IPA 插件
需要：[IPA插件管理器](https://github.com/Eusth/IPA)（请先按照其安装说明进行安装！）。

 1. 从[发布页面](../../releases)下载 XUnity.AutoTranslator-IPA-{版本}.zip。
 2. 直接解压到游戏目录中，使插件dll文件放入Plugins文件夹。
 3. 启动游戏。
 4. 由于各种考虑，默认情况下并非所有文本钩子都被启用，因此如果您发现游戏或游戏的某些部分没有被正确翻译，可能值得进入配置文件并启用一些已禁用的文本框架！配置文件在游戏启动时创建。

文件结构应该像这样：
```
{游戏目录}/Plugins/XUnity.Common.dll
{游戏目录}/Plugins/XUnity.ResourceRedirector.dll
{游戏目录}/Plugins/XUnity.AutoTranslator.Plugin.Core.dll
{游戏目录}/Plugins/XUnity.AutoTranslator.Plugin.IPA.dll
{游戏目录}/Plugins/XUnity.AutoTranslator.Plugin.ExtProtocol.dll
{游戏目录}/Plugins/MonoMod.RuntimeDetour.dll
{游戏目录}/Plugins/MonoMod.Utils.dll
{游戏目录}/Plugins/Mono.Cecil.dll
{游戏目录}/Plugins/0Harmony.dll
{游戏目录}/Plugins/ExIni.dll
{游戏目录}/Plugins/Translators/{翻译器}.dll
{游戏目录}/Plugins/Translation/任意翻译文件.txt（这些文件将由插件自动生成！）
 ```

### UnityInjector 插件
需要：UnityInjector（请先按照其安装说明进行安装！）。

 1. 从[发布页面](../../releases)下载 XUnity.AutoTranslator-UnityInjector-{版本}.zip。
 2. 直接解压到游戏目录中，使插件dll文件放入UnityInjector文件夹。**这可能不是游戏根目录！**
 3. 启动游戏。
 4. 由于各种考虑，默认情况下并非所有文本钩子都被启用，因此如果您发现游戏或游戏的某些部分没有被正确翻译，可能值得进入配置文件并启用一些已禁用的文本框架！配置文件在游戏启动时创建。

文件结构应该像这样：
```
{游戏目录}/UnityInjector/XUnity.Common.dll
{游戏目录}/UnityInjector/XUnity.ResourceRedirector.dll
{游戏目录}/UnityInjector/XUnity.AutoTranslator.Plugin.Core.dll
{游戏目录}/UnityInjector/XUnity.AutoTranslator.Plugin.UnityInjector.dll
{游戏目录}/UnityInjector/XUnity.AutoTranslator.Plugin.ExtProtocol.dll
{游戏目录}/UnityInjector/0Harmony.dll
{游戏目录}/UnityInjector/Translators/{翻译器}.dll
{游戏目录}/UnityInjector/Config/Translation/任意翻译文件.txt（这些文件将由插件自动生成！）
 ```

**注意：** 此安装方法不支持MonoMod钩子，因为Sybaris使用了过时版本的`Mono.Cecil.dll`。
 
## 按键映射
以下按键输入已映射：
 * ALT + 0：切换XUnity AutoTranslator UI。（这是数字零，不是字母O）
 * ALT + 1：切换Translation Aggregator UI。
 * ALT + T：在此插件提供的所有文本的已翻译和未翻译版本之间切换。
 * ALT + R：重新加载翻译文件。如果您即时更改文本和纹理文件，这很有用。不保证对所有纹理都有效。
 * ALT + U：手动钩取。默认钩子不会总是捕获文本。这将尝试手动进行查找。不会钩取未启用框架的文本组件。
 * ALT + F：如果配置了OverrideFont，将在覆盖字体和默认字体之间切换。
 * ALT + Q：如果插件被关闭则重新启动。这只有在插件因向翻译端点的连续错误而关闭时才有效。只有当您有理由相信您已经解决了问题（例如更改了VPN端点等）时才应该使用，否则它只会再次关闭。

仅用于调试的按键：
  * CTRL + ALT + NP9：模拟同步错误
  * CTRL + ALT + NP8：模拟延迟一秒的异步错误
  * CTRL + ALT + NP7：将已加载的场景名称和ID打印到控制台
  * CTRL + ALT + NP6：将整个GameObject层次结构打印到文件`hierarchy.txt`

## 翻译器
翻译是通过翻译端点获得的，这些端点本质上是AutoTranslator的插件。端点插件存储在`Translators`子文件夹中。

### 内置翻译器
以下是开箱即用支持的翻译器列表：
 * [GoogleTranslate](https://untrack.link/https://translate.google.com/)，基于在线Google翻译服务。不需要身份验证。
   * 无限制，但不稳定。
 * [GoogleTranslateV2](https://untrack.link/https://translate.google.com/)，基于在线Google翻译服务。不需要身份验证。
   * 无限制，但不稳定。目前正在测试中。可能会在将来替换原始版本，因为该API不再在其官方翻译网站上使用。
 * [GoogleTranslateCompat](https://untrack.link/https://translate.google.com/)，与上述相同，除了请求是在进程外提供的，这在某些版本的Unity/Mono中是必需的。
   * 无限制，但不稳定。
 * [GoogleTranslateLegitimate](https://untrack.link/https://cloud.google.com/translate/)，基于Google云翻译API。需要API密钥。
   * 提供1年试用期，$300积分。足够翻译1500万字符。
 * [BingTranslate](https://untrack.link/https://www.bing.com/translator)，基于在线Bing翻译服务。不需要身份验证。
   * 无限制，但不稳定。
 * [BingTranslateLegitimate](https://untrack.link/https://docs.microsoft.com/en-us/azure/cognitive-services/translator/translator-info-overview)，基于Azure文本翻译。需要API密钥。
   * 每月免费200万字符。
 * [DeepLTranslate](https://untrack.link/https://www.deepl.com/translator)，基于在线DeepL翻译服务。不需要身份验证。
   * 无限制，但不稳定。质量卓越。
 * [DeepLTranslateLegitimate](https://untrack.link/https://www.deepl.com/translator)，基于在线DeepL翻译服务。需要API密钥。
   * 每月4.99美元，当月翻译的每百万字符20美元。
   * 每月免费50万字符。
   * 目前，您必须订阅DeepL API（开发者版）。- 不适用于DeepL Pro（入门版、高级版和旗舰版）
 * [PapagoTranslate](https://untrack.link/https://papago.naver.com/)，基于在线Papago翻译服务。不需要身份验证。
   * 无限制，但不稳定。
 * [BaiduTranslate](https://untrack.link/https://fanyi.baidu.com/)，基于百度翻译服务。需要AppId和AppSecret。
   * 注册后，每月前5万字符免费（QPS=1），之后按49元/百万字符收费。如果您通过了免费身份认证，那么每月前100万字符免费（QPS=10），超出部分按49元/百万字符收费。单次请求最长6000字符。
 * [YandexTranslate](https://untrack.link/https://tech.yandex.com/translate/)，基于Yandex翻译服务。需要API密钥。
   * 每天免费100万字符，但每月最多1000万字符。
 * [WatsonTranslate](https://untrack.link/https://cloud.ibm.com/apidocs/language-translator)，基于IBM的Watson。需要URL和API密钥。
   * 每月免费100万字符。
 * LecPowerTranslator15，基于LEC的Power Translator。不需要身份验证，但需要安装该软件。
   * 无限制。
 * ezTrans XP，基于Changsinsoft的日韩翻译器ezTrans XP。不需要身份验证，但需要安装该软件和[Ehnd](https://github.com/sokcuri/ehnd)。
   * 无限制。
 * [LingoCloudTranslate](https://untrack.link/https://fanyi.caiyunapp.com/)，基于在线彩云翻译服务。仅支持中文和另外两种语言：日语和英语的翻译。
   * 注册并免费认证后，每月前100万字符免费，超出部分按20元/百万字符收费。官方测试令牌为`3975l6lr5pcbvidl6jl2`，您可以在注册前试用。
 * CustomTranslate。您还可以指定任何可以用作翻译端点的自定义HTTP URL（GET请求）。这必须使用查询参数"from"、"to"和"text"，并且只返回包含结果的字符串（首先尝试不使用SSL的HTTP，因为unity-mono经常在SSL方面遇到问题）。
   * *注意：这是一个以开发者为中心的选项。您不能简单地指定"CustomTranslate"并期望它与您在线找到的任何任意翻译服务一起工作。请参阅[常见问题](#常见问题)*
   * 示例配置：
     * Endpoint=CustomTranslate
     * [Custom]
     * Url=http://my-custom-translation-service.net/translate
   * 示例请求：GET http://my-custom-translation-service.net/translate?from=ja&to=en&text=こんにちは
   * 示例响应（仅正文）：Hello
   * 可以与CustomTranslate一起使用的已知实现：
     * ezTrans：https://github.com/HelloKS/ezTransWeb

*注意：如果您使用任何不需要某种形式身份验证的在线翻译器，该插件可能随时中断。*

### 第三方翻译器
自3.0.0版本起，您也可以实现自己的翻译器。要做到这一点，请按照[这里](#实现翻译器)的说明进行操作。
以下是一些您可以与AutoTranslator一起使用的第三方翻译插件：
 * [SugoiOfflineTranslatorEndpoint](https://github.com/Vin-meido/XUnity-AutoTranslator-SugoiOfflineTranslatorEndpoint)，用于Sugoi Translator服务器。
   * 无限制。质量卓越。
 * [LlmTranslators](https://github.com/joshfreitas1984/XUnity.AutoTranslate.LlmTranslators)，用于OpenAI的LLM和Ollama模型。
   * OpenAI需要API密钥，按使用的令牌付费。本地托管的Ollama模型免费。
 * [AutoChatGptTranslator](https://github.com/joshfreitas1984/XUnity.AutoChatGptTranslator)，用于ChatGPT。已过时，请改用LlmTranslators。
   * 需要API密钥，按使用的令牌付费。
 * [AutoLLMTranslator](https://github.com/NothingNullNull/XUnity.AutoLLMTranslator)，支持多种不同LLM（包括Ollama模型）的通用端点。
   * 非常灵活，但需要高级手动配置。仅推荐给高级用户。

*注意：您使用第三方插件的风险自负 - 它们在被添加到列表时已经过检查，但可能会随时间发生变化。第三方插件可能导致问题或存在安全问题。*
 
### 关于认证翻译器
如果您决定使用认证服务，*永远不要共享您的密钥或秘密*。如果您意外这样做了，您应该立即撤销它。大多数（如果不是全部）服务都提供了此选项。

如果您想使用付费选项，请记住在付费之前检查该插件是否支持您要翻译的源语言和目标语言。此外，虽然插件确实尝试将发送到翻译端点的请求数量保持在最低限度，但无法保证它会要求端点翻译多少内容，并且此存储库的作者/所有者对您因使用此插件而从您选择的翻译提供商那里收到的任何费用不承担任何责任。

插件尝试最小化发送请求数量的方法在[这里](#防垃圾机制)概述。

### 防垃圾机制
插件采用以下防垃圾机制：
 1. 当它看到新文本时，它总是会等待一秒钟再排队翻译请求，以检查相同的文本是否发生变化。在文本1秒钟内没有变化之前，它不会发送任何请求。
 2. 在单个游戏会话中，它永远不会发送超过8000个请求（每个最多200个字符（可配置））。
 3. 它永远不会同时发送超过1个请求（无并发！）。
 4. 如果检测到排队翻译数量增加（4000个），插件将关闭。
 5. 如果服务连续五次请求都没有返回结果，插件将关闭。
 6. 如果插件检测到游戏每帧都在排队翻译，插件将在90帧后关闭。
 7. 如果插件检测到文本"滚动"到位，插件将关闭。这是通过检查所有排队翻译的请求来检测的。（（1）通常会防止这种情况发生）
 8. 如果插件连续60秒以上每秒都排队翻译，插件将关闭。
 9. 对于支持的语言，每个可翻译的行必须通过符号检查，该检查检测行是否包含源语言的字符。
 10. 它永远不会尝试翻译已经被视为其他内容翻译的文本。
 11. 所有排队的翻译都被跟踪。如果两个不同的组件需要相同的翻译并且同时排队翻译，只会发送一个请求。
 12. 它使用内部手动翻译词典（总共约2000个）来处理常用短语（仅限日语到英语），以防止为这些短语发送翻译请求。
 13. 某些端点支持翻译批处理，因此发送的请求要少得多。这不会增加每个会话的翻译总数（2）。
 14. 所有翻译结果都缓存在内存中并存储在磁盘上，以防止进行相同的翻译请求两次。
 15. 由于其垃圾特性，来自IMGUI组件的任何文本都会将其中发现的数字模板化（并在翻译时替换回去），以防止与（6）相关的问题。
 16. 插件将保持一个TCP连接到翻译端点。如果50秒内未使用，该连接将被优雅关闭。

## 文本框架
支持以下文本框架：
 * [UGUI](https://docs.unity3d.com/Manual/UISystem.html)
 * [NGUI](https://assetstore.unity.com/packages/tools/gui/ngui-next-gen-ui-2413)
 * [IMGUI](https://docs.unity3d.com/Manual/GUIScriptingGuide.html)（默认禁用）
 * [TextMeshPro](http://digitalnativestudios.com/textmeshpro/docs/)
 * [TextMesh](https://docs.unity3d.com/Manual/class-TextMesh.html)（默认禁用，通常文本在3D空间中浮动）
 * [FairyGUI for Unity](https://github.com/fairygui/FairyGUI-unity)
 * [Utage (VN Game Engine)](http://madnesslabo.net/utage/?lang=en)

## 配置
默认配置文件如下所示：

```ini
[Service]
Endpoint=GoogleTranslate         ;要使用的端点。有效值请参阅[翻译器部分](#翻译器)。
FallbackEndpoint=                ;如果主端点在特定翻译中失败，自动回退到的端点。

[General]
Language=en                      ;要翻译到的语言
FromLanguage=ja                  ;游戏的原始语言。某些端点也支持"auto"，但通常不推荐使用

[Files]
Directory=Translation\{Lang}\Text                                   ;搜索缓存翻译文件的目录。可以使用占位符：{GameExeName}，{Lang}
OutputFile=Translation\{Lang}\Text\_AutoGeneratedTranslations.txt   ;插入生成的翻译的文件。可以使用占位符：{GameExeName}，{Lang}
SubstitutionFile=Translation\{Lang}\Text\_Substitutions.txt         ;包含在翻译前应用的替换的文件。可以使用占位符：{GameExeName}，{Lang}
PreprocessorsFile=Translation\{Lang}\Text\_Preprocessors.txt        ;包含在向翻译器发送文本前应用的预处理器的文件。可以使用占位符：{GameExeName}，{Lang}
PostprocessorsFile=Translation\{Lang}\Text\_Postprocessors.txt      ;包含在从翻译器接收文本后应用的后处理器的文件。可以使用占位符：{GameExeName}，{Lang}

[TextFrameworks]
EnableUGUI=True                  ;启用或禁用UGUI翻译
EnableNGUI=True                  ;启用或禁用NGUI翻译
EnableTextMeshPro=True           ;启用或禁用TextMeshPro翻译
EnableTextMesh=False             ;启用或禁用TextMesh翻译
EnableIMGUI=False                ;启用或禁用IMGUI翻译

[Behaviour]
MaxCharactersPerTranslation=200  ;每个文本翻译的最大字符数。最大2500。
IgnoreWhitespaceInDialogue=True  ;是否忽略对话键中的空白字符，包括换行符
IgnoreWhitespaceInNGUI=True      ;是否忽略NGUI中的空白字符，包括换行符
MinDialogueChars=20              ;文本被视为对话的长度
ForceSplitTextAfterCharacters=0  ;一旦翻译的文本超过此字符数，将文本拆分为多行
CopyToClipboard=False            ;是否将钩取的文本复制到剪贴板
MaxClipboardCopyCharacters=450   ;一次钩取到剪贴板的最大字符数
ClipboardDebounceTime=1.25       ;钩取文本到达剪贴板需要的秒数。最小值为0.1
EnableUIResizing=True            ;插件是否应该在翻译时提供"最佳尝试"调整UI组件大小
EnableBatching=True              ;指示是否应该为支持的端点启用翻译批处理
UseStaticTranslations=True       ;指示是否使用包含的静态翻译缓存中的翻译
OverrideFont=                    ;覆盖更新文本组件时用于文本的字体。注意：仅适用于UGUI
OverrideFontTextMeshPro=         ;考虑使用FallbackFontTextMeshPro代替。覆盖更新文本组件时用于文本的字体。注意：仅适用于TextMeshPro
FallbackFontTextMeshPro=         ;为TextMeshPro添加回退字体，以防不支持特定字符。这比OverrideFontTextMeshPro更推荐
ResizeUILineSpacingScale=        ;UI调整大小时默认行间距应该缩放的十进制值，例如：0.80。注意：仅适用于UGUI
ForceUIResizing=True             ;指示是否应该将UI调整大小行为应用于所有UI组件，无论它们是否被翻译。
IgnoreTextStartingWith=\u180e;   ;指示插件应该忽略以某些字符开头的任何字符串。这是一个由';'分隔的列表。
TextGetterCompatibilityMode=False ;指示是否启用"文本获取器兼容模式"。只有在游戏需要时才应该启用。
GameLogTextPaths=                ;指示游戏用作"日志组件"的游戏对象的特定路径，在这些路径中游戏会连续附加或前置文本。需要专业知识设置。这是一个由';'分隔的列表。
RomajiPostProcessing=ReplaceMacronWithCircumflex;RemoveApostrophes;ReplaceHtmlEntities ;指示对'翻译的'罗马字文本进行何种后处理。这在某些游戏中很重要，因为使用的字体不能正确支持各种变音符号。这是一个由';'分隔的列表。可能的值：["RemoveAllDiacritics", "ReplaceMacronWithCircumflex", "RemoveApostrophes", "ReplaceHtmlEntities"]
TranslationPostProcessing=ReplaceMacronWithCircumflex;ReplaceHtmlEntities ;指示对翻译文本（非罗马字）进行何种后处理。可能的值：["RemoveAllDiacritics", "ReplaceMacronWithCircumflex", "RemoveApostrophes", "ReplaceWideCharacters", "ReplaceHtmlEntities"]
RegexPostProcessing=None         ;指示对正则表达式的捕获组进行何种后处理。可能的值：["RemoveAllDiacritics", "ReplaceMacronWithCircumflex", "RemoveApostrophes", "ReplaceWideCharacters", "ReplaceHtmlEntities"]
CacheRegexLookups=False          ;指示是否应该将正则表达式查找的结果输出到指定的OutputFile
CacheWhitespaceDifferences=False ;指示是否应该将空白字符差异输出到指定的OutputFile
CacheRegexPatternResults=False   ;指示是否应该将正则表达式拆分翻译的完整结果输出到指定的OutputFile
GenerateStaticSubstitutionTranslations=False ;指示插件在使用替换时应该生成不带变量的翻译
GeneratePartialTranslations=False ;指示插件应该生成部分翻译以支持文本翻译"滚动入"
EnableTranslationScoping=False   ;指示插件应该解析'TARC'指令并基于这些指令限定翻译范围
EnableSilentMode=False           ;指示插件不应该打印与翻译相关的成功消息
BlacklistedIMGUIPlugins=         ;如果IMGUI窗口程序集/类/方法名称包含此列表中的任何字符串（不区分大小写），则该UI将不会被翻译。需要MonoMod钩子。这是一个由';'分隔的列表
OutputUntranslatableText=False   ;指示是否应该将插件认为不可翻译的文本输出到指定的OutputFile
IgnoreVirtualTextSetterCallingRules=False; 指示在尝试设置文本组件的文本时应该忽略虚拟方法调用的规则。在某些情况下可能有助于设置顽固组件的文本
MaxTextParserRecursion=1         ;指示当文本被解析以便在不同部分翻译时允许多少级别的递归。这可以与分割器正则表达式一起在高级场景中使用。默认值1本质上意味着递归被禁用。
HtmlEntityPreprocessing=True     ;将在发送翻译前预处理和解码html实体。某些翻译器在发送html实体时会失败。
HandleRichText=True              ;将启用富文本（带标记的文本）的自动处理
PersistRichTextMode=Final        ;指示应该如何持久化解析的富文本。"Fragment"表示分片存储文本，"Final"表示存储整个翻译字符串（不支持替换！）
EnableTranslationHelper=False    ;指示是否应该启用翻译器相关的有用日志消息。在基于重定向资源翻译时可能有用
ForceMonoModHooks=False          ;指示插件必须使用MonoMod钩子而不是harmony钩子
InitializeHarmonyDetourBridge=False ;指示插件应该初始化harmony detour bridge，允许harmony钩子在System.Reflection.Emit不存在的环境中工作（通常这些设置由插件管理器处理，所以使用插件管理器时不要使用）
RedirectedResourceDetectionStrategy=AppendMongolianVowelSeparatorAndRemoveAll ;指示插件是否以及如何尝试识别重定向资源以防止重复翻译。可以是["None", "AppendMongolianVowelSeparator", "AppendMongolianVowelSeparatorAndRemoveAppended", "AppendMongolianVowelSeparatorAndRemoveAll"]
OutputTooLongText=False          ;指示插件是否应该输出超过'MaxCharactersPerTranslation'的文本而不翻译它

[Texture]
TextureDirectory=Translation\{Lang}\Texture ;转储纹理并从中加载图像的目录根目录。可以使用占位符：{GameExeName}，{Lang}
EnableTextureTranslation=False   ;指示插件是否尝试用TextureDirectory目录中的图像替换游戏内图像
EnableTextureDumping=False       ;指示插件是否将其能够替换的纹理转储到TextureDirectory。会显著影响性能
EnableTextureToggling=False      ;指示使用ALT+T热键切换翻译是否也会影响纹理。不保证对所有纹理都有效。会显著影响性能
EnableTextureScanOnSceneLoad=False ;指示插件是否应该在场景加载时扫描纹理。这使插件能够找到并（可能）替换更多纹理
EnableSpriteRendererHooking=False ;指示插件是否应该尝试钩取SpriteRenderer。这是一个单独的选项，因为SpriteRenderer实际上无法被正确钩取，而实现的变通方法可能在某些情况下对性能产生理论影响
LoadUnmodifiedTextures=False     ;指示是否应该加载未修改的纹理。修改是基于文件名中的哈希值确定的。只在调试时启用，因为它可能导致奇怪的现象
TextureHashGenerationStrategy=FromImageName ;指示模组如何通过哈希值识别图片。可以是["FromImageName", "FromImageData", "FromImageNameAndScene"]
DuplicateTextureNames=           ;指示游戏中重复的特定纹理名称。列表由';'分隔。
DetectDuplicateTextureNames=False;指示插件是否应该检测重复的纹理名称。
EnableLegacyTextureLoading=False ;指示插件应该使用不同的策略加载图像，如果游戏引擎较旧，这可能是相关的
CacheTexturesInMemory=True       ;指示所有加载的纹理都应该保留在内存中以获得最佳性能。禁用以减少内存使用

[ResourceRedirector]
PreferredStoragePath=Translation\{Lang}\RedirectedResources ;指示与自动翻译器相关的重定向资源的首选存储。可以使用占位符：{GameExeName}，{Lang}
EnableTextAssetRedirector=False  ;指示是否应该重定向TextAssets
LogAllLoadedResources=False      ;指示插件是否应该将所有加载的资源记录到控制台。有助于确定什么可以被钩取
EnableDumping=False              ;指示是否应该转储找到的可翻译资源
CacheMetadataForAllFiles=True    ;当文件位于PreferredStoragePath中的ZIP文件中时，这些文件在内存中被索引以避免在加载时执行文件检查IO。启用此选项将对物理文件做同样的事情

[Http]
UserAgent=                       ;覆盖需要用户代理的API使用的用户代理
DisableCertificateValidation=False ;指示是否应该禁用.NET API的证书验证

[TranslationAggregator]
Width=400                        ;翻译聚合器窗口的总宽度。
Height=100                       ;翻译聚合器窗口的宽度（每个翻译器）。
EnabledTranslators=              ;在翻译聚合器窗口中启用的翻译端点的ID。列表由';'分隔。

[Google]
ServiceUrl=                      ;可选，可用于将Google API请求定向到不同的URL。可用于绕过GFWoC

[GoogleLegitimate]
GoogleAPIKey=                    ;可选，如果配置了GoogleTranslateLegitimate则需要

[BingLegitimate]
OcpApimSubscriptionKey=          ;可选，如果配置了BingTranslateLegitimate则需要

[Baidu]
BaiduAppId=                      ;可选，如果配置了BaiduTranslate则需要
BaiduAppSecret=                  ;可选，如果配置了BaiduTranslate则需要

[Yandex]
YandexAPIKey=                    ;可选，如果配置了YandexTranslate则需要

[Watson]
Url=                             ;可选，如果配置了WatsonTranslate则需要
Key=                             ;可选，如果配置了WatsonTranslate则需要

[DeepL]
MinDelay=2                       ;可选，用于限制DeepL
MaxDelay=7                       ;可选，用于限制DeepL

[DeepLLegitimate]
ApiKey=                          ;可选，如果配置了DeepLLegitimate则需要
Free=False                       ;可选，如果配置了DeepLLegitimate则需要

[Custom]
Url=                             ;可选，如果配置了CustomTranslated则需要

[LecPowerTranslator15]
InstallationPath=                ;可选，如果配置了LecPowerTranslator15则需要

[LingoCloud]
LingoCloudToken=                 ;可选，如果配置了LingoCloudTranslate则需要

[Debug]
EnableConsole=False              ;启用控制台。如果其他插件（管理器）处理此功能，请不要启用
EnableLog=False                  ;启用额外的日志记录以用于调试目的

[Migrations]
Enable=True                      ;用于启用此配置文件的自动迁移
Tag=4.15.0                        ;表示此插件最后执行的版本的标签。请勿编辑
```

### Behaviour Configuration Explanation

#### Whitespace Handling
This section describes configuration parameters that has an effect on whitespace handling before and after performing a translation. **None of these settings have an impact on the 'untranslated texts' that are placed in the auto generated translations file.**

When it comes to automated translations, proper whitespace handling can really make or break the translation. The parameters that control whitespace handling are:
 * `IgnoreWhitespaceInDialogue`
 * `IgnoreWhitespaceInNGUI`
 * `MinDialogueChars`
 * `ForceSplitTextAfterCharacters`

The plugin first determines whether or not it should perform a special whitespace removal operation. It determines whether or not to perform this operation based on the parameters `IgnoreWhitespaceInDialogue`, `IgnoreWhitespaceInNGUI` and `MinDialogueChars`:
 * `IgnoreWhitespaceInDialogue`: If the text is longer than `MinDialogueChars`, whitespace is removed.
 * `IgnoreWhitespaceInNGUI`: If the text comes from an NGUI component, whitespace is removed.

After the text has been translated by the configured service, `ForceSplitTextAfterCharacters` is used to determine if the plugin should force the result into multiple lines after a certain number of characters.

The main reason that this type of handling can make or break a translation really comes down to whether or not whitespace is removed from the source text before sending it to the endpoint. Most endpoints (such as GoogleTranslate) consider text on multiple lines seperately, which can often result in terrible translation if an unnecessary newline is included.

#### Text post/pre-processing
While proper whitespace handling goes a long way in ensuring better translations, it is not always enough.

The `PreprocessorsFile` allows defining entries that modifies the text just before it is sent to the translator.

The `PostprocessorsFile` allows defining entries that modifies the translated text just after it is received from the translator.

#### UI Resizing
Often when performing a translation on a text component, the resulting text is larger than the original. This often means that there is not enough room in the text component for the result. This section describes ways to remedy that by changing important parameters of the text components.

By default, the plugin will attempt some basic auto-resizing behaviour, which are controlled by the following parameters: `EnableUIResizing`, `ResizeUILineSpacingScale`, `ForceUIResizing`, `OverrideFont` and `OverrideFontTextMeshPro`.
 * `EnableUIResizing`: Resizes the components when a translation is performed.
 * `ForceUIResizing`: Resizes all components at all times, period.
 * `ResizeUILineSpacingScale`: Changes the line spacing of resized components. UGUI only.
 * `OverrideFont`: Changes the font of all text components regardless of `EnableUIResizing` and `ForceUIResizing`. UGUI only.
 * `OverrideFontTextMeshPro`: Consider using `FallbackFontTextMeshPro` instead. Changes the font of all text components regardless of `EnableUIResizing` and `ForceUIResizing`. TextMeshPro only. This option is able to load a font in two different ways. If the specified string indicates a path within the game folder, then that file will be attempted to be loaded as an asset bundle (requires Unity 2018 or greater (or alternatively a custom asset bundle built specifically for the targeted game)). If not, it will be attempted to be loaded through the Resources API. Default resources that are often distributed with TextMeshPro are: `Fonts & Materials/LiberationSans SDF` or `Fonts & Materials/ARIAL SDF`.
 * `FallbackFontTextMeshPro`: Adds a fallback font that TextMesh Pro can use in case a specific character is not supported.

An additional note on changing the font of TextMeshPro: You can download some pre-built asset bundles for Unity 2018 and 2019 in the release tab, but for now, they are not particularly well tested. If you want to try them out, simply download the .zip folder and put one of the font assets into the game folder. Then configure it up by writing the name of the file in the configuration file in `OverrideFontTextMeshPro`.

Resizing of a UI component does not refer to changing of it's dimensions, but rather how the component handles overflow. The plugin changes the overflow parameters such that text is more likely to be displayed.

The configuratiaon `EnableUIResizing` and `ForceUIResizing` also control whether or not manual UI resize behaviour is enabled. See [this section](#ui-font-resizing) for more information.

#### Reducing Translation Requests
The following aims at reducing the number of requests send to the translation endpoint:
 * `EnableBatching`: Batches several translation requests into a single with supported endpoints.
 * `UseStaticTranslations`: Enables usage of internal lookup dictionary of various english-to-japanese terms.
 * `MaxCharactersPerTranslation`: Specifies the maximum length of a text to translate. Any texts longer than this is ignored by the plugin. Cannot be greater than 1000. **Never redistribute this mod with this value greater than 400**

#### Romaji 'translation'
One of the possible values as output `Language` is 'romaji'. If you choose this as language, you will find that games often has problems showing the translations because the font does not understand the special characters used, for example the [macron diacritic](https://en.wikipedia.org/wiki/Macron_(diacritic)).

To rememdy this, post processing can be applied to translations when 'romaji' is chosen as `Language`. This is done through the option `RomajiPostProcessing`. This option is a ';'-seperated list of values:
 * `RemoveAllDiacritics`: Remove all diacritics from the translated text
 * `ReplaceMacronWithCircumflex`: Replaces the macron diacritic with a circumflex.
 * `RemoveApostrophes`: Some translators might decide to include apostrophes after the 'n'-character. Applying this option removes those.
 * `ReplaceWideCharacters`: Replaces wide-width japanese characters with standard ASCII characters
 * `ReplaceHtmlEntities`: Replaces all html entities with their unescaped character

This type of post processing is also applied to normal translations, but instead uses the option `TranslationPostProcessing`, which can use the same values.

#### MonoMod Hooks
MonoMod hooks are hooks are created at runtime, but not through the Harmony dependency. Harmony has two primary problems that these hooks attempt to solve:
 * Harmony cannot hook methods with no body.
 * Harmony cannot hook methods under the `netstandard2.0` API surface, which later versions of Unity can be build under.

MonoMod solves both of these problems. In order to use MonoMod hooks the libraries `MonoMod.RuntimeDetours.dll`, `MonoMod.Utils.dll` and `Mono.Cecil.dll` must be available to the plugin. These are optional dependencies.

These are only available in the following packages:
 * `XUnity.AutoTranslator-BepInEx-{VERSION}.zip` (because all dependencies are distributed with BepInEx 5.x)
 * `XUnity.AutoTranslator-IPA-{VERSION}.zip` (because all dependencies are included in the package)
 * `XUnity.AutoTranslator-ReiPatcher-{VERSION}.zip` (because all dependencies are included in the package)

They are not distributed in the BepInEx 4.x because of the potential for conflicts in mod packages for various games.

The following configuration controls the MonoMod hooks:
 * `ForceMonoModHooks`: Forces the plugin to use MonoMod hooks over Harmony hooks.

If MonoMod hooks are not forced they are only used if available and a given method cannot be hooked through Harmony for one of the two reasons mentioned above.

#### Other Options
 * `TextGetterCompatibilityMode`: This mode fools the game into thinking that the text displayed is not translated. This is required if the game uses text displayed to the user to determine what logic to execute. You can easily determine if this is required if you can see the functionality works fine if you toggle the translation off (hotkey: ALT+T).
 * `IgnoreTextStartingWith`: Disable translation for any texts starting with values in this ';-separated' setting. The [default value](https://www.charbase.com/180e-unicode-mongolian-vowel-separator) is an invisible character that takes up no space.
 * `CopyToClipboard`: Copy text to translate to the clipboard to support tools such as Translation Aggregator.
 * `ClipboardDebounceTime`: The delay between hooking a text and it being copied to clipboard. This is to avoid spamming the clipboard. If multiple texts appear in this period they will be concatenated.
 * `EnableSilentMode`: Indicates the plugin should not print out success messages in relation to translations.
 * `BlacklistedIMGUIPlugins`: If an IMGUI window assembly/class/method name contains any of the strings in this list (case insensitive) that UI will not be translated. Requires MonoMod hooks. This is a list seperated by ';'.
 * `OutputUntranslatableText`: Indicates if texts that are considered by the plugin to be untranslatable should be output to the specified OutputFile. Enabling this may also output a lot of garbage to the `OutputFile` that should be deleted before potential redistribution. **Never redistribute the mod with this enabled.**
 * `IgnoreVirtualTextSetterCallingRules`: Indicates that rules for virtual method calls should be ignored when trying to set the text of a text component. May in some cases help setting the text of stubborn components.
 * `RedirectedResourceDetectionStrategy`: Indicates if and how the plugin should attempt to recognize redirected resources in order to prevent double translations. Can be ["None", "AppendMongolianVowelSeparator", "AppendMongolianVowelSeparatorAndRemoveAppended", "AppendMongolianVowelSeparatorAndRemoveAll"]
 * `OutputTooLongText`: Indicates if the plugin should output text that exceeds 'MaxCharactersPerTranslation' without translating it

## IL2CPP 支持
虽然这个插件提供了一定程度的IL2CPP支持，但绝不是完整的。可以观察到以下差异/缺失功能：
 * 文本钩取能力不佳
 * 不支持TextGetterCompatibilityMode
 * 不支持特定插件的翻译（尚未支持）
 * 不支持IMGUI翻译（尚未支持）
 * 许多其他功能完全未经验证

## 常见问题
> **问：如何禁用自动翻译？**  
答：按ALT+0时选择空端点或将配置参数`Endpoint=`设置为空。

> **问：如何完全禁用插件？**  
答：您可以通过删除"{游戏目录}\BepInEx\plugins"目录中的"XUnity.AutoTranslator"目录来完成。避免删除"XUnity.ResourceRedirector"目录，因为其他插件可能依赖它。

> **问：当此插件应用翻译时，游戏停止工作。**  
答：尝试设置以下配置参数`TextGetterCompatibilityMode=True`。

> **问：这个插件能翻译其他插件/模组吗？**  
答：很可能可以，请参阅[这里](#翻译模组)。

> **问：如何使用CustomTranslate？**  
答：如果您必须问这个问题，那么您可能无法使用它。CustomTranslate是为翻译服务开发者准备的。他们能够公开符合CustomTranslate API规范的API，而无需在此插件中实现自定义ITranslateEndpoint。

> **问：请提供对翻译服务X的支持。**  
答：目前，对不需要某种形式身份验证的服务的额外支持不太可能。但请注意，可以独立于此插件实现自定义翻译器。而且这样做需要的代码非常少。

## 翻译模组
通常其他模组的UI是通过IMGUI实现的。如您所见，这默认是禁用的。通过将"EnableIMGUI"值更改为"True"，它也会开始翻译IMGUI，这可能意味着其他模组的UI也会被翻译。

还可以提供特定插件的翻译。请参阅下一节。

## 手动翻译
当您使用此插件时，您始终可以转到文件`Translation\{Lang}\Text\_AutoGeneratedTranslations.txt`（OutputFile）来编辑任何自动生成的翻译，它们将在您下次运行游戏时显示。或者您可以按（ALT+R）立即重新加载翻译。

还值得注意的是，此插件将读取`Translation`（目录）中的所有文本文件（*.txt），因此如果您想提供手动翻译，您可以简单地从`Translation\_AutoGeneratedTranslations.{lang}.txt`（OutputFile）中剪切文本并将它们放在新的文本文件中，以便用手动翻译替换它们。这些文本文件也可以放在标准的.zip存档中。

在这种情况下，`Translation\{Lang}\Text\_AutoGeneratedTranslations.txt`（OutputFile）在读取翻译时始终具有最低优先级。因此，如果同一翻译出现在两个地方，不会使用来自（OutputFile）的翻译。

在某些ADV引擎中，文本会缓慢地"滚动"到位。为此使用了不同的技术，在某些情况下，如果您希望翻译文本滚动进入而不是未翻译文本，您可能需要设置`GeneratePartialTranslations=True`。除非游戏需要，否则不应该启用此选项。

### Plugin-specific Manual Translations
Often you may want to provide translations for other plugins that are not naturally translated. This is obviously also possible with this plugin as described in the previous section. But what if you want to provide translations that should be specific to that plugin because such translation would conflict with a different plugin/generic translation?

In order to add plugin-specific translations, simply create a `Plugins` directory in the text translation `Directory`. In this directory you can create a new directory for each plugin you want to provide plugin-specific translations for. The name of the directory should be the same as the dll name without the extension (.dll).

Within this directory you can create translations files as you normally would. In addition you can add the following directive in these files:

```
#enable fallback
```

This will allow the plugin-specific translations to fallback to the generic/automated translations provided by the plugin. It does not matter which translation file this directive is placed it and it only need to be added once.

As a plugin author it is also possible to embed these translation files in your plugin and register them through code with the following API:

```csharp
/// <summary>
/// Entry point for manipulating translations that have been loaded by the plugin.
///
/// Methods on this interface should be called during plugin initialization. Preferably during the Start callback.
/// </summary>
public static class TranslationRegistry
{
    /// <summary>
    /// Obtains the translations registry instance.
    /// </summary>
    public static ITranslationRegistry Default { get; }
}

/// <summary>
/// Interface for manipulating translation that have been loaded by the plugin.
/// </summary>
public interface ITranslationRegistry
{
    /// <summary>
    /// Registers and loads the specified translation package.
    /// </summary>
    /// <param name="assembly">The assembly that the behaviour should be applied to.</param>
    /// <param name="package">Package containing translations.</param>
    void RegisterPluginSpecificTranslations( Assembly assembly, StreamTranslationPackage package );

    /// <summary>
    /// Registers and loads the specified translation package.
    /// </summary>
    /// <param name="assembly">The assembly that the behaviour should be applied to.</param>
    /// <param name="package">Package containing translations.</param>
    void RegisterPluginSpecificTranslations( Assembly assembly, KeyValuePairTranslationPackage package );

    /// <summary>
    /// Allow plugin-specific translation to fallback to generic translations.
    /// </summary>
    /// <param name="assembly">The assembly that the behaviour should be applied to.</param>
    void EnablePluginTranslationFallback( Assembly assembly );
}
```

### Substitutions
It is also possible to add substitutions that are applied to found texts before translations are created. This is controlled through the `SubstitutionFile`, which uses the same format as normal translation text files, although things like regexes are not supported.

This is useful for replacing names that are often translated incorrectly, etc.

When using substitutions, the found occurrences will be parameterized in the generated translations, like so:

```
私は{{A}}=I am {{A}}
```

Alternatively, if the configuration `GenerateStaticSubstitutionTranslations=True` is used the translations will not be parameterized.

When creating manual translations, use this file as sparingly as you would use regexes, as it can have an effect on performance.

*NOTE: If the text to be translated includes rich text, it cannot currently be parameterized.*

### Regex Usage
Text translation files support regexes as well. Always remember to use regexes sparingly and scope them to avoid performance issues.

Regexes can be applied to translations in two different ways. The following two sections describes these two ways:

#### Standard Regex Translation
Standard regex translation are simply regexes that applied directly onto a translatable text, if no direct lookup can be found.

```
r:"^シンプルリング ([0-9]+)$"=Simple Ring $1
```

These are identified by the untranslated text starting with 'r:'.

#### Splitter Regex
Sometimes games likes to combine texts before displaying them on screen. This means that it can sometimes be hard to know what text to add to the translation file because it appears in a number of different ways.

This section explores a solution to this by applying a regex to split the text to be translated into individual pieces before trying to make lookups for the specified texts.

For example, let's say an accessory (Simple Ring) would be translated with the following line `シンプルリング=Simple Ring`. Now lets say this appears in multiple textboxes throughout the game like `01 シンプルリング` and `02 シンプルリング`. Providing a standard regex in a translation file to handle this is not going to work because you would need a regex for each accessory and this would not be performant at all.

However, if we split the translation before trying to make lookups it will allow us to only have a single simple translation in our file, like this: `シンプルリング=Simple Ring`.

Simply place the following regex in a translation file:

```
sr:"^([0-9]{2}) ([\S\s]+)$"=$1 $2
```

This will split up the text to be translated into two parts, translate them individually and put them back together.

These are identified by the untranslated text starting with 'sr:'.

It is also worth noting that this methodology can be used recursively, if configured. This means that it allows the individual strings that were split for translations by a regex, to flow into another splitter regex, and so on.

In addition to identifying each group by index, they can also be identified by a name, which allows groups to be completely additional. Let's take a look at an example that combines all of these things:

```
sr:"^\[(?<stat>[\w\s]+)(?<num_i>[\+\-]{1}[0-9]+)?\](?<after>[\s\S]+)?$"="[${stat}${num_i}]${after}"
```

In this example there are 3 named groups, two of which are optional (standard regex syntax). The replacement pattern identifies these named group by surrounding the name with `${}`.

If the identifier name ends in `_i` it means that the string will not be attempted to be translated, but rather transfered as is. Generally this is not really needed as the plugin is smart enough to determine if something should be translated or not.

So what would this regex split? It would split strings like this:

```
[DEF+14][ATK+64][DEX+34][AGI]
```

The group(s) `(?<stat>[\w]+)(?<num_i>[\+\-]{1}[0-9]+)?` matches the text inside the `[]`. As you can see there are two groups. The first is requried and represents the text. The second is optional and represents the plus-/minus sign and number that comes after.

The group `(?<after>[\s\S]+)` matches whatever comes after. Because of this, it will attempt to translate that text like any other, and that may flow directly back into this splitter regex.

#### Regex Post Processing
Using the configuration option `RegexPostProcessing`, it is also possible to apply post processing the to the groups of a regex. For `sr:` regexes they are only applied to groups where the identifier name ends in `_i`.

### UI Font Resizing
It is also possible to manually control the font size of text components. This is useful when the translated text uses more space than the untranslated text.

You can control this in files that end in `resizer.txt` placed in the translation `Directory`. This file takes a simply syntax like this:

```
CharaCustom/CustomControl/CanvasDraw=ChangeFontSizeByPercentage(0.5)
```

In these files, the left-hand side of the equals sign represents a (partial) path to the components that must have their fonts resized. The right-hand sized represent a ';'-separated list of the command to perform on those texts.

In the shown example it will reduce the font size of all texts below the specified path to 50%.

Like any other translation file, these files also support translation scoping, as decribed in [this section](#translation-scoping).

The following types of commands exists:
 * Commands that change the font size to a static size:
   * `ChangeFontSizeByPercentage(double percentage)`: Where the percentage is the percentage of the original font size to reduce it to.
   * `ChangeFontSize(int size)`: Where the size if the new size of the font
   * `IgnoreFontSize()`: This can be used to reset font resize behavior that was set on a very 'non-specific' path.
 * Commands that control auto-resizing:
   * `AutoResize(bool enabled, minSize, maxSize)`: Where enabled control if auto-resize behaviour should be enabled. The two last parameters are optional.
     * minSize, maxSize possible values: [keep, none, any number]
 * Commands that control the line spacing (UGUI only):
   * `UGUI_ChangeLineSpacingByPercentage(float percentage)`
   * `UGUI_ChangeLineSpacing(float lineSpacing)`
 * Commands that control horizontal overflow (UGUI only):
   * `UGUI_HorizontalOverflow(string mode)` - possible values: [wrap, overflow]
 * Commands that control vertical overflow (UGUI only):
   * `UGUI_VerticalOverflow(string mode)` - possible values: [truncate, overflow]
 * Commands to control overflow (TMP only):
   * `TMP_Overflow(string mode)` - [possible values](https://docs.unity3d.com/Packages/com.unity.textmeshpro@3.0/api/TMPro.TextOverflowModes.html)
 * Commands to control text alignment (TMP only):
   * `TMP_Alignment(string mode)` - [possible values](https://docs.unity3d.com/Packages/com.unity.textmeshpro@3.0/api/TMPro.TextAlignmentOptions.html)

But stop you say! How would I determine the path to use? This plugin provides no way to easily determine this, but there are other plugins that will allow you to do this.

There's two ways, and you will likely need to use both of them:
 * Using [Runtime Unity Editor](https://github.com/ManlyMarco/RuntimeUnityEditor) to determine these.
 * Enabling the option `[Behaviour] EnableTextPathLogging=True`, which will log out the path to all text components that text are changed on.

### Translation Scoping
The following two options are available when it comes to scoping translations to only part of the game:

The translation files support the following directives:
 * `#set level 1,2,3` tells the plugin that translations following this line in this file may only be applied in scenes with ID 1, 2 or 3.
 * `#unset level 1,2,3` tells the plugin that translations following this line in this file should not be applied in scenes with ID 1, 2 or 3. If no levels are set, all specified translations are global.
 * `#set exe game1,game2` tells the plugin that translations following this line in this file may only be applied when the game is run through an executable with the name game1 or game2.
 * `#unset exe game1,game2` tells the plugin that translations following this line in this file should not be applied when the game is run through an executable with the name game1 or game2. If no exes are set, all specified translations are global.
 * `#set required-resolution height > 1280 && width > 720` tells the plugin that translations following this line in this file should only be applied if the resolution is greater than specified. Current implementation only handles the resolution used by the game at startup.
 * `#unset required-resolution` tells the plugin to ignore previously specified `#set required-resolution` directive.

For this to work, the following configuration option must be `True`:

```
[Behaviour]
EnableTranslationScoping=True
```

Also, this behaviour is not available in the `OutputFile`.

You can always see which levels are loaded by using the hotkey CTRL+ALT+NP7.

Another way of scoping translations are through file names. It is possible to tell the plugin where to look for translation files. It is possible to parameterize these paths with the variable {GameExeName}.

Example configuration that seperates translations for each executable:

```
[Files]
Directory=Translation\{GameExeName}\{Lang}\Text
Directory=Translation\{GameExeName}\{Lang}\Text\_AutoGeneratedTranslations.txt
Directory=Translation\{GameExeName}\{Lang}\Text\_Substitutions.txt
```

So when should use scope your translations? Well that depends on the type of scope:
 * `level` scopes should really only be used to avoid translation collisions
 * `exe` scopes can be used both to avoid translation collisions and to enhance performance

### Text Lookup and Whitespace Handling
This section is provided to give the translator an understanding of how this plugin looks up texts and provides translations.

In the simplest form, the way the plugin works is as a dictionary of untranslated text strings. When plugin sees a text that it considers untranslated, it will attempt to look up the text string in the dictionary and if it finds a result, it will display the found translation instead.

The world, however, is not always that simple. Depending on the engine/text framework used by a game, an untranslated text string may be slightly different when used in different contexts. For example for a VN it may not be the exact same text string that appears in the "ADV history"-view as it was when it was being initially displayed to the user.

**Example:**
```
「こう見えて怒っているんですよ？……失礼しますね」
「こう見えて怒っているんですよ？\n ……失礼しますね」
```

These text strings are not the same and it would be annoying having to translate the same text multiple times if the final translation is supposed to be the same. 

In fact, only one of these translations are needed. Here's why: (still very much simplified):
 1. When the plugin sees an untranslated text, it will actually make four lookups, not one. These are, in order:
    * Based on the untouched original text
    * Based on the original text but without leading/trailing whitespace. If found the leading/trailing whitespace is added to the resulting translation
    * Based on the original text but without internal non-repeating whitespace surrounding a newline
    * Based on the original text but without leading/trailing whitespace and internal non-repeating whitespace surrounding a newline. If found the leading/trailing whitespace is added to the resulting translation

This means that for the following string `\n 「こう見えて怒っているんですよ？\n ……失礼しますね」` the plugin will make the following lookups:
```
\n 「こう見えて怒っているんですよ？\n ……失礼しますね」
「こう見えて怒っているんですよ？\n ……失礼しますね」
\n 「こう見えて怒っているんですよ？……失礼しますね」
「こう見えて怒っているんですよ？……失礼しますね」
```

 2. When the plugin loads the (manual/automatic) translation it will not make one dictionary entry, but three. These are:
    * Based on the untouched original text and original translation
    * Based on the original text (without leading/trailing whitespace) and original translation (without leading/trailing whitespace)
    * Based on the original text (without leading/trailing whitespace and internal non-repeating whitespace surrounding a newline) and original translation (without leading/trailing whitespace and internal non-repeating whitespace surrounding a newline)
   
This means that for the following string `\n 「こう見えて怒っているんですよ？\n ……失礼しますね」` the plugin will make the following entries:
```
\n 「こう見えて怒っているんですよ？\n ……失礼しますね」
「こう見えて怒っているんですよ？\n ……失礼しますね」
「こう見えて怒っているんですよ？……失礼しますね」
```

This means you can get away with providing a single translation for both of these cases. Which you think is better is up to you.

Another thing to note is that the plugin will always output the original text without modifications in the translation file. But if it sees another text afterwards that is "compatible" with that text-string (due to the above mentioned text modifications) it will not output this new text by default.

This is controlled by the configuration option `CacheWhitespaceDifferences=False`. You can change this to true, and it will output a new entry for each unique text, even if the only differences are whitespace. Obviously, translations-pairs actually appearing in the translation file will always that precendent over translations-pairs that are generated based on an exinsting translation-pair.

*NOTE: Whitespace differences in relation to level-scoped translations will never be output regardless of this setting.*

### Resource Redirection
Sometimes it's easier to provide a translation to a game by directly overriding the game resource files. However, directly overriding the game resource files is also problematic because that means the modification will likely only work for one version of the game.

To overcome this problem, and allow for modification of resource files, this plugin also has a resource redirector module that allows redirecting any kind of resource loaded by the game.

Before we get into the details of this module, it is worth mentioning that it is:
 * It is not a plugin. Rather it is just a library that is not beholden to any plugin manager (it does come with a plugin-compatible BepInEx DLL but this is only to manage configuration).
 * It is game independent.
 * And while it may be redistributed with the Auto Translator, it is completely independent from it and it can be used without having the Auto Translator installed.

The DLLs required for the Resource Redirector to work are `XUnity.Common.dll` and `XUnity.ResourceRedirector.dll`. By themselves, these libraries do nothing.

By default the Auto Translator plugin comes with one resource redirector for `TextAsset`, which basically outputs the raw text assets to the file system allowing them to be individually overridden.

More redirectors can be implemented for specific games, though this does require programming knowledge, see [this section](#implementing-a-resource-redirector) for more information.

The Auto Translator has the following Resource Redirector-specific configuration:
 * `PreferredStoragePath`: Indicates where the Auto Translator should store redirected resources.
 * `EnableTextAssetRedirector`: Indicates if the TextAsset redirector is enabled.
 * `LogAllLoadedResources`: Indicates if Resource Redirector should log all resources to the console (can also be controlled through Resource Redirector API surface).
 * `EnableDumping`: Indicates if resources redirected to the Auto Translator should be dumped for overwriting if possible.
 * `CacheMetadataForAllFiles`: When files are in ZIP files in the PreferredStoragePath, these files are indexed in memory to avoid performing file check IO when loading them. Enabling this option will do the same for physical files

ZIP files that are placed in the `PreferredStoragePath` will be indexed during startup, allowing redirected resources to be compressed and zipped. When files are placed in a zip file, the zip file is simply treated as not existing during file lookup.

## 关于重新分发
绝对鼓励为各种游戏重新分发此插件。但是，如果您这样做，请牢记以下几点：
 * **在重新分发时分发_AutoGeneratedTranslations.txt文件，并尽可能多地包含翻译，以确保尽可能少地访问在线翻译器。**
 * **在启用日志记录/控制台的情况下测试您的重新分发，以确保游戏不会表现出不良行为，例如向端点发送垃圾信息。**
 * 不要使用来自此存储库的非默认翻译端点配置重新分发插件。这意味着：
   * 不要设置`Endpoint=DeepLTranslate`然后重新分发。
   * 但是，如果您实现了自己的端点或端点不是此存储库的一部分，您可以继续将其作为默认端点重新分发。
 * 确保您尽可能保持插件更新。
 * 如果您使用图像加载功能，请确保您阅读[此部分](#纹理翻译)。

## 纹理翻译
从版本2.16.0+开始，此模组提供了替换图像的基本功能。这是一个默认禁用的功能。但是，这些图像不会自动翻译。

此功能主要适用于几乎没有模组支持的游戏，以便在不需要修改资源文件的情况下启用完整翻译。

它由以下配置控制：

```ini
[Texture]
TextureDirectory=Translation\Texture
EnableTextureTranslation=False
EnableTextureDumping=False
EnableTextureToggling=False
EnableTextureScanOnSceneLoad=False
EnableSpriteRendererHooking=False
LoadUnmodifiedTextures=False
TextureHashGenerationStrategy=FromImageName
DuplicateTextureNames=
DetectDuplicateTextureNames=False
EnableLegacyTextureLoading=False
CacheTexturesInMemory=True
```

`TextureDirectory` specifies the directory where textures are dumped to and loaded from. Loading will happen from all subdirectories of the specified directory as well, so you can move dumped images to whatever folder structure you desire.

`EnableTextureTranslation` enables texture translation. This basically means that textures will be loaded from the `TextureDirectory` and it's subsdirectories. These images will replace the in-game images used by the game.

`EnableTextureDumping` enables texture dumping. This means that the mod will dump any images it has not already dumped to the `TextureDirectory`. When dumping textures, it may also be worth enabling `EnableTextureScanOnSceneLoad` to more quickly find all textures that require translating. **Never redistribute the mod with this enabled.**

`EnableTextureScanOnSceneLoad` allows the plugin to scan for texture objects on the sceneLoad event. This enables the plugin to find more texture at a tiny performance cost during scene load (which is often during loading screens, etc.). However, because of the way Unity works not all of these are guaranteed to be replacable. If you find an image that is dumped but cannot be translated, please report it. However, please recognize this mod is primarily intended for replacing UI textures, not textures for 3D meshes.

`EnableSpriteRendererHooking` allows the plugin to attempt to hook SpriteRenderer. This is a seperate option because SpriteRenderer can't actually be hooked properly and the implemented workaround could have a theoretical impact on performance in certain situations.

`LoadUnmodifiedTextures` enables whether or not the plugin should load textures that has not been modified. This is only useful for debugging, and likely to cause various visual glitches, especially if `EnableTextureScanOnSceneLoad` is also enabled. **Never redistribute the mod with this enabled.**

`EnableTextureToggling` enables whether the ALT+T hotkey will also toggle textures. This is by no means guaranteed to work, especially if `EnableTextureScanOnSceneLoad` is also enabled. **Never redistribute the mod with this enabled.**

`DuplicateTextureNames` specifies different textures in the game that are used under the same resource name. The plugin will fallback to the 'FromImageData' for image identification for these images.

`DetectDuplicateTextureNames` specifies that the plugin should identify which image names are duplicated and update the configuration with these names automatically. **Never redistribute the mod with this enabled.**

`EnableLegacyTextureLoading` specifies that the plugin should use attempt to load images differently, which may be relevant if the unity engine is old (verified with versions less than 5.3). This should not be used unless the images that are loaded are not the ones that you expected.

`CacheTexturesInMemory` specifies that all translation textures should be kept in memory to optimize performance. Can be disabled to reduce memory usage.

`TextureHashGenerationStrategy` specifies how images are identified. When images are stored, the game will need some way of associating them with the image that it has to replace.
This is done through a hash-value that is stored in square brackets in each image file name, like this: `file_name [0223B639A2-6E698E9272].png`. This configuration specifies how these hash-values are generated:
 * `FromImageName` means that the hash is generated from the internal resource name that the game uses for the image, which may not exist for all images or even be unique. However, it is generally fairly reliable. If an image has no resource name, it will not be dumped.
 * `FromImageData` means that the hash is generated from the data stored in the image, which is guaranteed to exist for all images. However, generating the hash comes at a performance cost, that will also be incurred by the end-users.
 * `FromImageNameAndScene` means that it should use the name and scene to generate a hash. The name is still required for this to work. When using this option, there is a chance the same texture could be dumped with different hashes, which is undesirable, but it could be required for some games, if the name itself is not unique and the `FromImageData` option causes performance issues. If this is used, it is recommended to enable `EnableTextureScanOnSceneLoad` as well.

There's an important catch you need to be aware when dealing with these options and that is if ANY of these options exists: `EnableTextureDumping=True`, `EnableTextureToggling=True`, `TextureHashGenerationStrategy=FromImageData`, then the game will need to read the raw data from all images it finds in game in order to replace the image and this is an expensive operation.

It is therefore recommended to use `TextureHashGenerationStrategy=FromImageName`. Most likely, images without a resource name won't be interesting to translate anyway.

If you redistribute this mod with translated images, it is recommended you delete all images you either have no intention of translating or are not translated at all.

You can also change the file name to whatever you desire, as long as you keep the hash appended to the end of the file name.

If you take anything away from this section, it should be these two points:
 * **Never redistribute the mod with `EnableTextureDumping=True`, `EnableTextureToggling=True`, `LoadUnmodifiedTextures=True` or `DetectDuplicateTextureNames=true`**
 * **Only redistribute the mod with `TextureHashGenerationStrategy=FromImageData` enabled if absolutely required by the game.**

### Technical details about Hash Generation in file names
There are actually two hashes in the generated file name, separated by a dash (-):
 * The first hash is a SHA1 (only first 5 bytes) based on the `TextureHashGenerationStrategy` used. If `FromImageName` is specified, then it is based on the UTF8 (without BOM) representation.
 * The second hash is a SHA1 (only first 5 bytes) based on the data in the image. This is used to determine whether or not the image has been modified, so images that has not been edited are not loaded. Unless `LoadUnmodifiedTextures` is specified.

If `TextureHashGenerationStrategy=FromImageData` is specified, only a single hash will appear in each file name, as that single hash can be used both to identify the image and to determine whether or not it has been edited.

## Integrating with Auto Translator
*NOTE: Everything below this point requires programming knowledge!*

### Implementing a plugin that can query translations
As a mod author, you may want to query translations from the plugin. This easily done, take a look at the example below.

```C#
public class MyPlugin : XPluginBase
{
   public void Start()
   {
      var untranslatedText = "お前はもう死んでいる！";

      // EXAMPLE 1) Query cache, and if it cannot be found in cache query the user-selected translation endpoint
      AutoTranslator.Default.TranslateAsync( untranslatedText, result =>
      {
         if( result.Succeeded )
         {
            var translatedText = result.TranslatedText;
         }
         else
         {
            var errorMessage = result.ErrorMessage;
         }
      } );

      // EXAMPLE 2) Query cache only
      if( AutoTranslator.Default.TryTranslate( untranslatedText, out string translation ) )
      {
         // success
      }
      else
      {
         // failure
      }
   }
}
```

This requires version 3.7.0 or later!

### Implementing a component that the Auto Translator should not interfere with
As a mod author, you might not want the Auto Translator to interfere with your mods UI. If this is the case there's two ways to tell Auto Translator not to perform any translation:
 * If your UI is based on GameObjects, you can simply name your GameObjects containing the text element (for example Text class) to something that contains the string "XUAIGNORE". The Auto Translator will check for this and ignore components that contains the string.
 * If your UI is based on IMGUI, the above approach is not possible, because there are no GameObject. In that case you can do the following instead:

```C#
public class MyPlugin : XPluginBase
{
   private GameObject _xua;
   private bool _lookedForXua;

   public void OnGUI()
   {
      // make sure we only do this lookup once, as it otherwise may be detrimental to performance!
      // also: do not attempt to do this in the Awake method or similar of your plugin, as your plugin may be instantiated before the auto translator!
      if(!_lookedForXua)
      {
         _lookedForXua = true;
         _xua = GameObject.Find( "___XUnityAutoTranslator" );
      }

      // try-finally block is important to make sure you re-enable the plugin
      try
      {
         _xua?.SendMessage("DisableAutoTranslator");

         // do your GUI things here
         GUILayout.Button( "こんにちは！" );
      }
      finally
      {
         _xua?.SendMessage("EnableAutoTranslator");
      }
   }
}
```

This requires version 2.15.0 or later!

## Implementing a Translator
Since version 3.0.0, you can now also implement your own translators.

In order to do so, all you have to do is implement the following interface, build the assembly and place the generated DLL in the `Translators` folder.

```C#
/// <summary>
/// The interface that must be implemented by a translator.
/// </summary>
public interface ITranslateEndpoint
{
   /// <summary>
   /// Gets the id of the ITranslateEndpoint that is used as a configuration parameter.
   /// </summary>
   string Id { get; }

   /// <summary>
   /// Gets a friendly name that can be displayed to the user representing the plugin.
   /// </summary>
   string FriendlyName { get; }

   /// <summary>
   /// Gets the maximum concurrency for the endpoint. This specifies how many times "Translate"
   /// can be called before it returns.
   /// </summary>
   int MaxConcurrency { get; }

   /// <summary>
   /// Gets the maximum number of translations that can be served per translation request.
   /// </summary>
   int MaxTranslationsPerRequest { get; }

   /// <summary>
   /// Called during initialization. Use this to initialize plugin or throw exception if impossible.
   /// </summary>
   void Initialize( IInitializationContext context );

   /// <summary>
   /// Attempt to translated the provided untranslated text. Will be used in a "coroutine",
   /// so it can be implemented in an asynchronous fashion.
   /// </summary>
   IEnumerator Translate( ITranslationContext context );
}
```

Often an implementation of this interface will access an external web service. If this is the case, you do not need to implement the entire interface yourself. Instead you can rely on a base class in the `XUnity.AutoTranslator.Plugin.Core` assembly. But more on this later.

### Important Notes on Implementing a Translator based on an Online Service
Whenever you implement a translator based on an online service, it is important to not use it in an abusive way. For example by:
 * Establishing a large number of connections to it
 * Performing web scraping instead of using an available API
 * Making concurrent requests towards it
 * *This is especially important if the service is not authenticated*

With that in mind, consider the following:
 * The `WWW` class in Unity establishes a new TCP connection on each request you make, making it extremely poor at this kind of job. Especially if SSL (https) is involved because it has to do the entire handshake procedure each time. Yuck.
 * The `UnityWebRequest` class in Unity does not exist in most games, because the authors use an old engine, so it is not a good choice either.
 * The `WebClient` class from .NET is capable of using persistent connections (it does so by default), but has its own problems with SSL. The version of Mono used in most Unity games rejects all certificates by default making all HTTPS connections fail. This, however, can be remedied during the initialization phase of the translator (see examples below). Another shortcoming of this API is the fact that the runtime will never release the TCP connections it has used until the process ends. The API also integrates terribly with Unity because callbacks return on a background thread.
 * The `WebRequest` class from .NET is essentially the same as WebClient.
 * The `HttpClient` class from .NET is also unlikely to exist in most Unity games.

None of these are therefore an ideal solution.

To remedy this, the plugin implements a class `XUnityWebClient`, which is based on Mono's version of WebClient. However, it adds the following features:
 * Enables integration with Unity by returning result classes that can be 'yielded'.
 * Properly closes connections that has not been used for 50 seconds.

I recommend using this class, or in case that cannot be used, falling back to the .NET 'WebClient'.

### How-To
Follow these steps:
 1. Download XUnity.AutoTranslator-Developer-{VERSION}.zip from [releases](../../releases)
 2. Start a new project (.NET 3.5) in Visual Studio 2017 or later. I recommend using the same name for your assembly/project as the "Id" you are going to use in your interface implementation. This makes it easier for users to know how to configure your translator
    * I recommend using the "Class Library (.NET Standard)" and simply editing the generated .csproj file to use 'net35' instead of 'netstandard2.0'. This generates much cleaner .csproj files.
 3. Add a reference to the XUnity.AutoTranslator.Plugin.Core.dll that you downloaded in step 1
 4. You do not need to directly reference the UnityEngine.dll assembly. This is good, because you do not need to worry about which version of Unity is used.
    * If you do need a reference to this assembly (because you need functionality from it) consider using an old version of it (if `UnityEngine.CoreModule.dll` exists in the Managed folder, it is not an old version!)
 5. Create a new class that either:
    * Implements the `ITranslateEndpoint` interface
    * Inherits from the `HttpEndpoint` class
    * Inherits from the `WwwEndpoint` class
    * Inherits from the `ExtProtocolEndpoint` class

Here's an example that simply reverses the text and also reads some configuration from the configuration file the plugin uses:

```C#
public class ReverseTranslatorEndpoint : ITranslateEndpoint
{
   private bool _myConfig;

   public string Id => "ReverseTranslator";

   public string FriendlyName => "Reverser";

   public int MaxConcurrency => 50;

   public int MaxTranslationsPerRequest => 1;

   public void Initialize( IInitializationContext context )
   {
      _myConfig = context.GetOrCreateSetting( "Reverser", "MyConfig", true );
   }

   public IEnumerator Translate( ITranslationContext context )
   {
      var reversedText = new string( context.UntranslatedText.Reverse().ToArray() );
      context.Complete( reversedText );

      return null;
   }
}
```

Arguably, this is not a particularly interesting example, but it illustrates the basic principles of what must be done in order to implement a Translator.

Let's take a look at a more advanced example that accesses the web:

```C#
internal class YandexTranslateEndpoint : HttpEndpoint
{
   private static readonly HashSet<string> SupportedLanguages = new HashSet<string> { "az", "sq", "am", "en", "ar", "hy", "af", "eu", "ba", "be", "bn", "my", "bg", "bs", "cy", "hu", "vi", "ht", "gl", "nl", "mrj", "el", "ka", "gu", "da", "he", "yi", "id", "ga", "it", "is", "es", "kk", "kn", "ca", "ky", "zh", "ko", "xh", "km", "lo", "la", "lv", "lt", "lb", "mg", "ms", "ml", "mt", "mk", "mi", "mr", "mhr", "mn", "de", "ne", "no", "pa", "pap", "fa", "pl", "pt", "ro", "ru", "ceb", "sr", "si", "sk", "sl", "sw", "su", "tg", "th", "tl", "ta", "tt", "te", "tr", "udm", "uz", "uk", "ur", "fi", "fr", "hi", "hr", "cs", "sv", "gd", "et", "eo", "jv", "ja" };
   private static readonly string HttpsServicePointTemplateUrl = "https://translate.yandex.net/api/v1.5/tr.json/translate?key={3}&text={2}&lang={0}-{1}&format=plain";

   private string _key;

   public override string Id => "YandexTranslate";

   public override string FriendlyName => "Yandex Translate";

   public override void Initialize( IInitializationContext context )
   {
      _key = context.GetOrCreateSetting( "Yandex", "YandexAPIKey", "" );
      context.DisableCertificateChecksFor( "translate.yandex.net" );

      // if the plugin cannot be enabled, simply throw so the user cannot select the plugin
      if( string.IsNullOrEmpty( _key ) ) throw new Exception( "The YandexTranslate endpoint requires an API key which has not been provided." );
      if( !SupportedLanguages.Contains( context.SourceLanguage ) ) throw new Exception( $"The source language '{context.SourceLanguage}' is not supported." );
      if( !SupportedLanguages.Contains( context.DestinationLanguage ) ) throw new Exception( $"The destination language '{context.DestinationLanguage}' is not supported." );
   }

   public override void OnCreateRequest( IHttpRequestCreationContext context )
   {
      var request = new XUnityWebRequest(
         string.Format(
            HttpsServicePointTemplateUrl,
            context.SourceLanguage,
            context.DestinationLanguage,
            WwwHelper.EscapeUrl( context.UntranslatedText ),
            _key ) );
         
      request.Headers[ HttpRequestHeader.Accept ] = "*/*";
      request.Headers[ HttpRequestHeader.AcceptCharset ] = "UTF-8";

      context.Complete( request );
   }

   public override void OnExtractTranslation( IHttpTranslationExtractionContext context )
   {
      var data = context.Response.Data;
      var obj = JSON.Parse( data );

      var code = obj.AsObject[ "code" ].ToString();
      if( code != "200" ) context.Fail( "Received bad response code: " + code );

      var token = obj.AsObject[ "text" ].ToString();
      var translation = JsonHelper.Unescape( token.Substring( 2, token.Length - 4 ) );

      if( string.IsNullOrEmpty( translation ) ) context.Fail( "Received no translation." );

      context.Complete( translation );
   }
}
```

This plugin extends from `HttpEndpoint`. Let's look at the three methods it overrides:
 * `Initialize` is used to read the API key the user has configured. In addition it calls `context.DisableCertificateChecksFor( "translate.yandex.net" )` in order to disable the certificate check for this specific hostname. If this is neglected, SSL will fail in most versions of Unity. Finally, it throws an exception if the plugin cannot be used with the specified configuration.
 * `OnCreateRequest` is used to construct the `XUnityWebRequest` object that will be sent to the external endpoint. The call to `context.Complete( request )` specifies the request to use.
 * `OnExtractTranslation` is used to extract the text from the response returned from the web server.

As you can see, the `XUnityWebClient` class is not even used. We simply specify a request object that the `HttpEndpoint` will use internally to perform the request.

After implementing the class, simply build the project and place the generated DLL file in the "Translators" directory of the plugin folder. That's it.

For more examples of implementations, you can simply take a look at this projects source code.

**NOTE**: If you implement a class based on the `HttpEndpoint` and you get an error where the web request is never completed, then it is likely due to the web server requiring Tls1.2. Unity-mono has issues with this spec and it will cause the request to lock up forever. The only solutions to this for now are:
 * Disable SSL, if you can. There are many situations where it is simply not possible to do this because the web server will simply redirect back to the HTTPS endoint.
 * Use the `WwwEndpoint` instead. I highly advice against this though, unless it is an authenticated endpoint.

Another way to implement a translator is to implement the `ExtProtocolEndpoint` class. This can be used to delegate the actual translation logic to an external process. Currently there is no documentation on this, but you can take a look at the LEC implementation, which uses it.

If instead, you use the interface directly, it is also possible to extend from MonoBehaviour to get access to all the normal lifecycle callbacks of Unity components.

## Implementing a Resource Redirector
The resource director allows you to modify resources loaded through the `Resources` and `AssetBundle` API as they are being loaded by the game.

The following API surface is made available by the Resource Redirector:

```C#
/// <summary>
/// Entrypoint to the resource redirection API.
/// </summary>
public static class ResourceRedirection
{
    /// <summary>
    /// Gets or sets a bool indicating if the resource redirector
    /// should log all loaded resources/assets to the console.
    /// </summary>
    public static bool LogAllLoadedResources { get; set; }

    /// <summary>
    /// Register an AssetLoading hook (prefix to loading an asset from an asset bundle).
    /// </summary>
    /// <param name="priority">The priority of the callback, the higher the sooner it will be called.</param>
    /// <param name="action">The callback.</param>
    public static void RegisterAssetLoadingHook( int priority, Action<AssetLoadingContext> action );

    /// <summary>
    /// Unregister an AssetLoading hook (prefix to loading an asset from an asset bundle).
    /// </summary>
    /// <param name="action">The callback.</param>
    public static void UnregisterAssetLoadingHook( Action<AssetLoadingContext> action );

    /// <summary>
    /// Register an AsyncAssetLoading hook (prefix to loading an asset from an asset bundle asynchronously).
    /// </summary>
    /// <param name="priority">The priority of the callback, the higher the sooner it will be called.</param>
    /// <param name="action">The callback.</param>
    public static void RegisterAsyncAssetLoadingHook( int priority, Action<AsyncAssetLoadingContext> action );

    /// <summary>
    /// Unregister an AsyncAssetLoading hook (prefix to loading an asset from an asset bundle asynchronously).
    /// </summary>
    /// <param name="action">The callback.</param>
    public static void UnregisterAsyncAssetLoadingHook( Action<AsyncAssetLoadingContext> action );

    /// <summary>
    /// Register an AsyncAssetLoading hook and AssetLoading hook (prefix to loading an asset from an asset bundle synchronously/asynchronously).
    /// </summary>
    /// <param name="priority">The priority of the callback, the higher the sooner it will be called.</param>
    /// <param name="action">The callback.</param>
    public static void RegisterAsyncAndSyncAssetLoadingHook( int priority, Action<IAssetLoadingContext> action );

    /// <summary>
    /// Unregister an AsyncAssetLoading hook and AssetLoading hook (prefix to loading an asset from an asset bundle synchronously/asynchronously).
    /// </summary>
    /// <param name="action">The callback.</param>
    public static void UnregisterAsyncAndSyncAssetLoadingHook( Action<IAssetLoadingContext> action );

    /// <summary>
    /// Register an AssetLoaded hook (postfix to loading an asset from an asset bundle (both synchronous and asynchronous)).
    /// </summary>
    /// <param name="behaviour">The behaviour of the callback.</param>
    /// <param name="priority">The priority of the callback, the higher the sooner it will be called.</param>
    /// <param name="action">The callback.</param>
    public static void RegisterAssetLoadedHook( HookBehaviour behaviour, int priority, Action<AssetLoadedContext> action );

    /// <summary>
    /// Unregister an AssetLoaded hook (postfix to loading an asset from an asset bundle (both synchronous and asynchronous)).
    /// </summary>
    /// <param name="action">The callback.</param>
    public static void UnregisterAssetLoadedHook( Action<AssetLoadedContext> action );

    /// <summary>
    /// Register an AssetBundleLoading hook (prefix to loading an asset bundle synchronously).
    /// </summary>
    /// <param name="priority">The priority of the callback, the higher the sooner it will be called.</param>
    /// <param name="action">The callback.</param>
    public static void RegisterAssetBundleLoadingHook( int priority, Action<AssetBundleLoadingContext> action );

    /// <summary>
    /// Unregister an AssetBundleLoading hook (prefix to loading an asset bundle synchronously).
    /// </summary>
    /// <param name="action">The callback.</param>
    public static void UnregisterAssetBundleLoadingHook( Action<AssetBundleLoadingContext> action );

    /// <summary>
    /// Register an AsyncAssetBundleLoading hook (prefix to loading an asset bundle asynchronously).
    /// </summary>
    /// <param name="priority">The priority of the callback, the higher the sooner it will be called.</param>
    /// <param name="action">The callback.</param>
    public static void RegisterAsyncAssetBundleLoadingHook( int priority, Action<AsyncAssetBundleLoadingContext> action );

    /// <summary>
    /// Unregister an AsyncAssetBundleLoading hook (prefix to loading an asset bundle asynchronously).
    /// </summary>
    /// <param name="action">The callback.</param>
    public static void UnregisterAsyncAssetBundleLoadingHook( Action<AsyncAssetBundleLoadingContext> action );

    /// <summary>
    /// Register an AsyncAssetBundleLoading hook and AssetBundleLoading hook (prefix to loading an asset bundle synchronously/asynchronously).
    /// </summary>
    /// <param name="priority">The priority of the callback, the higher the sooner it will be called.</param>
    /// <param name="action">The callback.</param>
    public static void RegisterAsyncAndSyncAssetBundleLoadingHook( int priority, Action<IAssetBundleLoadingContext> action );

    /// <summary>
    /// Unregister an AsyncAssetBundleLoading hook and AssetBundleLoading hook (prefix to loading an asset bundle synchronously/asynchronously).
    /// </summary>
    /// <param name="action">The callback.</param>
    public static void UnregisterAsyncAndSyncAssetBundleLoadingHook( Action<IAssetBundleLoadingContext> action );

    /// <summary>
    /// Register a ReourceLoaded hook (postfix to loading a resource from the Resources API (both synchronous and asynchronous)).
    /// </summary>
    /// <param name="behaviour">The behaviour of the callback.</param>
    /// <param name="priority">The priority of the callback, the higher the sooner it will be called.</param>
    /// <param name="action">The callback.</param>
    public static void RegisterResourceLoadedHook( HookBehaviour behaviour, int priority, Action<ResourceLoadedContext> action );

    /// <summary>
    /// Unregister a ReourceLoaded hook (postfix to loading a resource from the Resources API (both synchronous and asynchronous)).
    /// </summary>
    /// <param name="action">The callback.</param>
    public static void UnregisterResourceLoadedHook( Action<ResourceLoadedContext> action );

    /// <summary>
    /// Enables experimental hooks that allows returning an Asset instead of a Request from async prefix
    /// asset load operations.
    /// </summary>
    public static void EnableSyncOverAsyncAssetLoads();

    /// <summary>
    /// Disables all recursive behaviour in the plugin. This means that trying to load an asset
    /// using the hooked APIs will not trigger a callback back into the callback chain. This makes
    /// setting the correct priorities on callbacks much more important.
    ///
    /// This method should not be called lightly. It should not be something a single plugin randomly
    /// decides to call, but rather decision for how to use the ResourceRedirection API on a game-wide basis.
    /// </summary>
    public static void DisableRecursionPermanently();

    /// <summary>
    /// Creates an asset bundle hook that attempts to load asset bundles in the emulation directory
    /// over the default asset bundles if they exist.
    /// </summary>
    /// <param name="hookPriority">Priority of the hook.</param>
    /// <param name="emulationDirectory">The directory to look for the asset bundles in.</param>
    public static void EnableEmulateAssetBundles( int hookPriority, string emulationDirectory );

    /// <summary>
    /// Disable a previously enabled asset bundle emulation.
    /// </summary>
    public static void DisableEmulateAssetBundles();

    /// <summary>
    /// Creates an asset bundle hook that redirects asset bundles loads to an empty
    /// asset bundle if the file that is being loaded does not exist.
    /// </summary>
    /// <param name="hookPriority">Priority of the hook.</param>
    public static void EnableRedirectMissingAssetBundlesToEmptyAssetBundle( int hookPriority );

    /// <summary>
    /// Disable a previously enabled redirect missing asset bundles to empty asset bundle.
    /// </summary>
    public static void DisableRedirectMissingAssetBundlesToEmptyAssetBundle();
}

```

Let's attach some comments to this API.

### Asset Loading/Loaded Methods
The resource redirector comes with both postfix and prefix callbacks when loading an asset from the `AssetBundle` API.

The event callback chain looks like this `[AssetLoading / AsyncAssetLoading hooks] => [Original Method] => [AssetLoaded hooks]`. The `AssetLoaded` event handles postfixes for both synchronous and asynchronous loading of assets.

#### Asset Loading Methods
The methods `RegisterAssetLoadingHook( HookBehaviour behaviour, Action<AssetLoadingContext> action )` and `RegisterAsyncAssetLoadingHook( int priority, Action<AsyncAssetLoadingContext> action )` hooks into the `AssetBundle` API when loading assets.

These methods registers prefix callbacks, which means the assets themselves wont be loaded yet when they are called.

The callbacks take the types `AssetLoadingContext` and `AsyncAssetLoadingContext` as an argument, respectively. Let's take a look at their definitions:

```C#
/// <summary>
/// The operation context surrounding the AssetLoading hook (synchronous).
/// </summary>
public class AssetLoadingContext : IAssetLoadingContext
{
    /// <summary>
    /// Gets the original path the asset bundle was loaded with.
    /// </summary>
    /// <returns>The unmodified, original path the asset bundle was loaded with.</returns>
    public string GetAssetBundlePath();

    /// <summary>
    /// Gets the normalized path to the asset bundle that is:
    ///  * Relative to the current directory
    ///  * Lower-casing
    ///  * Uses '\' as separators.
    /// </summary>
    /// <returns></returns>
    public string GetNormalizedAssetBundlePath();

    /// <summary>
    /// Indicate your work is done and if any other hooks to this asset/resource load should be called.
    /// </summary>
    /// <param name="skipRemainingPrefixes">Indicate if the remaining prefixes should be skipped.</param>
    /// <param name="skipOriginalCall">Indicate if the original call should be skipped. If you set the asset, you likely want to set this to true.</param>
    /// <param name="skipAllPostfixes">Indicate if the postfixes should be skipped.</param>
    public void Complete( bool skipRemainingPrefixes = true, bool? skipOriginalCall = true, bool? skipAllPostfixes = true );

    /// <summary>
    /// Disables recursive calls if you make an asset/asset bundle load call
    /// from within your callback. If you want to prevent recursion this should
    /// be called before you load the asset/asset bundle.
    /// </summary>
    public void DisableRecursion();

    /// <summary>
    /// Gets the original parameters the asset load call was called with.
    /// </summary>
    public AssetLoadingParameters Parameters { get; }

    /// <summary>
    /// Gets the AssetBundle associated with the loaded assets.
    /// </summary>
    public AssetBundle Bundle { get; }

    /// <summary>
    /// Gets or sets the loaded assets.
    ///
    /// Consider using this if the load type is 'LoadByType' or 'LoadNamedWithSubAssets'.
    /// </summary>
    public UnityEngine.Object[] Assets { get; set; }

    /// <summary>
    /// Gets or sets the loaded assets. This is simply equal to the first index of the Assets property, with some
    /// additional null guards to prevent NullReferenceExceptions when using it.
    /// </summary>
    public UnityEngine.Object Asset { get; set; }
}

/// <summary>
/// The operation context surrounding the AsyncAssetLoading hook (asynchronous).
/// </summary>
public class AsyncAssetLoadingContext : IAssetLoadingContext
{
    /// <summary>
    /// Gets the original path the asset bundle was loaded with.
    /// </summary>
    /// <returns>The unmodified, original path the asset bundle was loaded with.</returns>
    public string GetAssetBundlePath();

    /// <summary>
    /// Gets the normalized path to the asset bundle that is:
    ///  * Relative to the current directory
    ///  * Lower-casing
    ///  * Uses '\' as separators.
    /// </summary>
    /// <returns></returns>
    public string GetNormalizedAssetBundlePath();

    /// <summary>
    /// Indicate your work is done and if any other hooks to this asset/resource load should be called.
    /// </summary>
    /// <param name="skipRemainingPrefixes">Indicate if the remaining prefixes should be skipped.</param>
    /// <param name="skipOriginalCall">Indicate if the original call should be skipped. If you set the asset, you likely want to set this to true.</param>
    /// <param name="skipAllPostfixes">Indicate if the postfixes should be skipped.</param>
    public void Complete( bool skipRemainingPrefixes = true, bool? skipOriginalCall = true, bool? skipAllPostfixes = true );

    /// <summary>
    /// Disables recursive calls if you make an asset/asset bundle load call
    /// from within your callback. If you want to prevent recursion this should
    /// be called before you load the asset/asset bundle.
    /// </summary>
    public void DisableRecursion();

    /// <summary>
    /// Gets the original parameters the asset load call was called with.
    /// </summary>
    public AssetLoadingParameters Parameters { get; }

    /// <summary>
    /// Gets the AssetBundle associated with the loaded assets.
    /// </summary>
    public AssetBundle Bundle { get; }

    /// <summary>
    /// Gets or sets the AssetBundleRequest used to load assets.
    /// </summary>
    public AssetBundleRequest Request { get; set; }

    /// <summary>
    /// Gets or sets the loaded assets.
    ///
    /// Consider using this if the load type is 'LoadByType' or 'LoadNamedWithSubAssets'.
    /// </summary>
    UnityEngine.Object[] Assets { get; set; }

    /// <summary>
    /// Gets or sets the loaded assets. This is simply equal to the first index of the Assets property, with some
    /// additional null guards to prevent NullReferenceExceptions when using it.
    /// </summary>
    UnityEngine.Object Asset { get; set; }

    /// <summary>
    /// Gets or sets how this load operation should be resolved.
    /// Setting the Asset/Assets/Request property will automatically update this value.
    /// </summary>
    public AsyncAssetLoadingResolve ResolveType { get; set; }
}
```

The only difference between these two contexts is that one has an `Asset/Assets` property you can set, while the other has a `Request` property you can set.

Now if you actually paid attention to what you were reading(!?), you would notice that both of the above context objects has an `Asset/Assets` property that can be set.

Under normal circumstances, however, you cannot use the `Assets/Asset` property on the the `AsyncAssetLoadingContext`. In order to be able to use these, you must first call `ResourceRedirection.EnableSyncOverAsyncAssetLoads` once during your initialization logic. This will allow you to set the asset directly so you don't have to go through the standard `AssetBundle` API to obtain a request object.

It is, however, recommended that if you can that you set the `Request` property instead of the `Assets/Asset` property as that will keep the operation asynchronous and not block the game while the asset is being loaded.

If you can handle the loading of the asset remember to call the `Complete` method to indicate your intentions regarding:
 * Whether the rest of the prefixes registered should be skipped.
 * Whether the original method should be skipped.
 * Whether all the postfixes should be skipped.

An important points to make here, is that there is both an `Asset` and an `Assets` property on the context object. These can be used interchangably, but an array will only ever be used if the following condition apply:
 * The `LoadType` in the `Parameters` property is `LoadByType` or `LoadNamedWithSubAssets`, which are the only types of resource loading that may return multiple resources.

Finally, if we take a look at the `Parameters` property of the context object, we will find the following definition:

```C#
/// <summary>
/// Class representing the original parameters of the load call.
/// </summary>
public class AssetLoadingParameters
{
    /// <summary>
    /// Gets or sets the name of the asset being loaded. Will be null if loaded through 'LoadMainAsset' or 'LoadByType'.
    /// </summary>
    public string Name { get; set; }

    /// <summary>
    /// Gets or sets the type that passed to the asset load call.
    /// </summary>
    public Type Type { get; set; }

    /// <summary>
    /// Gets the type of call that loaded this asset. If 'LoadByType' or 'LoadNamedWithSubAssets' is specified
    /// multiple assets may be returned if subscribed as 'OneCallbackPerLoadCall'.
    /// </summary>
    public AssetLoadType LoadType { get; }
}

/// <summary>
/// Enum representing the different ways an asset may be loaded.
/// </summary>
public enum AssetLoadType
{
    /// <summary>
    /// Indicates that this asset has been loaded as the 'mainAsset' in the AssetBundle API.
    /// </summary>
    LoadMainAsset,

    /// <summary>
    /// Indicates that this call is loading all assets of a specific type in an AssetBundle API.
    /// </summary>
    LoadByType,

    /// <summary>
    /// Indicates that this call is loading a specific named asset in the AssetBundle API.
    /// </summary>
    LoadNamed,

    /// <summary>
    /// Indicates that this call is loading a specific named asset and all those below it in the AssetBundle API.
    /// </summary>
    LoadNamedWithSubAssets
}
```

Another way to change the result of the asset load operation is to change the value of the `Name` and `Type` properties in the `Parameters` property. If you do this, you likely will not want to call the Complete method, as you will want the original method to still be called.

An important additional way to subscribe to the prefix asset loading operations are through the method `RegisterAsyncAndSyncAssetLoadingHook( int priority, Action<IAssetLoadingContext> action )`. This method will handle both async and sync asset loading operations. The `IAssetLoadingContext` is an interface implemented by both the `AssetLoadingContext` and `AsyncAssetLoadingContext`.

Do note, that if you want to use this method you must first call the method `EnableSyncOverAsyncAssetLoads()` to enable the hooks required for this to work.

#### Asset Loaded Methods
The method `RegisterAssetLoadedHook( HookBehaviour behaviour, Action<AssetLoadedContext> action )` hooks into the `AssetBundle` API in the UnityEngine. Any time an asset is loaded through this API a callback is sent to these hooks.

This API is a postfix hook to the `AssetBundle` API, which means that it is first called once the original asset has already been loaded, but is still replacable.

The `AssetLoadedContext` class has the following definition:

```C#
/// <summary>
/// The operation context surrounding the AssetLoaded hook.
/// </summary>
public class AssetLoadedContext : IAssetOrResourceLoadedContext
{
    /// <summary>
    /// Gets a bool indicating if this resource has already been redirected before.
    /// </summary>
    public bool HasReferenceBeenRedirectedBefore( UnityEngine.Object asset );

    /// <summary>
    /// Gets a file system path for the specfic asset that should be unique.
    /// </summary>
    /// <param name="asset"></param>
    /// <returns></returns>
    public string GetUniqueFileSystemAssetPath( UnityEngine.Object asset );

    /// <summary>
    /// Gets the original path the asset bundle was loaded with.
    /// </summary>
    /// <returns>The unmodified, original path the asset bundle was loaded with.</returns>
    public string GetAssetBundlePath();

    /// <summary>
    /// Gets the normalized path to the asset bundle that is:
    ///  * Relative to the current directory
    ///  * Lower-casing
    ///  * Uses '\' as separators.
    /// </summary>
    /// <returns></returns>
    public string GetNormalizedAssetBundlePath();

    /// <summary>
    /// Indicate your work is done and if any other hooks to this asset/resource load should be called.
    /// </summary>
    /// <param name="skipRemainingPostfixes">Indicate if any other hooks should be skipped.</param>
    public void Complete( bool skipRemainingPostfixes = true );

    /// <summary>
    /// Disables recursive calls if you make an asset/asset bundle load call
    /// from within your callback. If you want to prevent recursion this should
    /// be called before you load the asset/asset bundle.
    /// </summary>
    public void DisableRecursion();

    /// <summary>
    /// Gets the original parameters the asset load call was called with.
    /// </summary>
    public AssetLoadedParameters Parameters { get; }

    /// <summary>
    /// Gets the AssetBundle associated with the loaded assets.
    /// </summary>
    public AssetBundle Bundle { get; }

    /// <summary>
    /// Gets the loaded assets. Override individual indices to change the asset reference that will be loaded.
    ///
    /// Consider using this if the load type is 'LoadByType' or 'LoadNamedWithSubAssets' and you subscribed with 'OneCallbackPerLoadCall'.
    /// </summary>
    public UnityEngine.Object[] Assets { get; set; }

    /// <summary>
    /// Gets the loaded asset. This is simply equal to the first index of the Assets property, with some
    /// additional null guards to prevent NullReferenceExceptions when using it.
    /// </summary>
    public UnityEngine.Object Asset { get; set; }
}
```

The HookBehaviour is an enum with the following definition:

```C#
/// <summary>
/// Enum indicating how the resource redirector should treat the callback.
/// </summary>
public enum HookBehaviour
{
    /// <summary>
    /// Specifies that exactly one callback should be received per call to asset/resource load method.
    /// </summary>
    OneCallbackPerLoadCall = 1,

    /// <summary>
    /// Specifies that exactly one callback should be received per loaded asset/resources. This means
    /// that the 'Asset' property should be used over the 'Assets' property on the context object.
    /// Do note that when using this option, if no resources are returned by a load call, no callbacks
    /// will be received.
    /// </summary>
    OneCallbackPerResourceLoaded = 2
}
```

An important points to make here, is that there is both an `Asset` and an `Assets` property on the context object. These can be used interchangably, but an array will only ever be used if the following two conditions apply:
 * You've subscribed with `OneCallbackPerLoadCall`.
 * The `LoadType` in the `Parameters` property is `LoadByType` or `LoadNamedWithSubAssets`, which are the only types of resource loading that may return multiple resources.

In relation to this, it is worth mentioning that if a call to load assets returns 0 assets, you will not receive any callbacks if you subscribe through `OneCallbackPerResourceLoaded` where as if you subscribe through `OneCallbackPerLoadCall` you would still get your one callback.

If you update or replace the asset being loaded remember to call to `Complete` method to indicate your intentions regarding:
 * Whether the remaining postfixes should be called.

In addition, if we take a look at the `Parameters` property of the context object, we will find the following definition:

```C#
/// <summary>
/// Class representing the original parameters of the load call.
/// </summary>
public class AssetLoadedParameters
{
    /// <summary>
    /// Gets the name of the asset being loaded. Will be null if loaded through 'LoadMainAsset' or 'LoadByType'.
    /// </summary>
    public string Name { get; }

    /// <summary>
    /// Gets the type that passed to the asset load call.
    /// </summary>
    public Type Type { get; }

    /// <summary>
    /// Gets the type of call that loaded this asset. If 'LoadByType' or 'LoadNamedWithSubAssets' is specified
    /// multiple assets may be returned if subscribed as 'OneCallbackPerLoadCall'.
    /// </summary>
    public AssetLoadType LoadType { get; }
}

/// <summary>
/// Enum representing the different ways an asset may be loaded.
/// </summary>
public enum AssetLoadType
{
    /// <summary>
    /// Indicates that this asset has been loaded as the 'mainAsset' in the AssetBundle API.
    /// </summary>
    LoadMainAsset,

    /// <summary>
    /// Indicates that this call is loading all assets of a specific type in an AssetBundle API.
    /// </summary>
    LoadByType,

    /// <summary>
    /// Indicates that this call is loading a specific named asset in the AssetBundle API.
    /// </summary>
    LoadNamed,

    /// <summary>
    /// Indicates that this call is loading a specific named asset and all those below it in the AssetBundle API.
    /// </summary>
    LoadNamedWithSubAssets
}
```

It is also worth mentioning that these hooks handles both synchronous and asynchronous loading of assets.

Hooks subscribed through hook behaviour `OneCallbackPerLoadCall` will be called before hooks with the behaviour `OneCallbackPerResourceLoaded`.

### Resource Load Methods
The resource redirector comes only with postfix callbacks when loading an asset from the `Resources` API.

The event callback chain looks like this `[Original Method] => [ResourceLoaded hooks]`. The `ResourceLoaded` event handles postfixes for both synchronous and asynchronous loading of assets.

#### Resource Loaded Methods
The method `RegisterResourceLoadedHook( HookBehaviour behaviour, Action<ResourceLoadedContext> action )` hooks into the `Resources` API in the UnityEngine. Any time a resource is loaded through this API a callback is sent to these hooks.

This API is a postfix hook to the `Resources` API, which means that it is first called once the original asset has already been loaded, but is still replacable.

The `ResourceLoadedContext` class has the following definition:

```C#
/// <summary>
/// The operation context surrounding the ResourceLoaded hook.
/// </summary>
public class ResourceLoadedContext : IAssetOrResourceLoadedContext
{
    /// <summary>
    /// Gets a bool indicating if this resource has already been redirected before.
    /// </summary>
    public bool HasReferenceBeenRedirectedBefore( UnityEngine.Object asset );

    /// <summary>
    /// Gets a file system path for the specfic asset that should be unique.
    /// </summary>
    /// <param name="asset"></param>
    /// <returns></returns>
    public string GetUniqueFileSystemAssetPath( UnityEngine.Object asset );

    /// <summary>
    /// Indicate your work is done and if any other hooks to this asset/resource load should be called.
    /// </summary>
    /// <param name="skipRemainingPostfixes">Indicate if any other hooks should be skipped.</param>
    public void Complete( bool skipRemainingPostfixes = true );

    /// <summary>
    /// Disables recursive calls if you make an asset/asset bundle load call
    /// from within your callback. If you want to prevent recursion this should
    /// be called before you load the asset/asset bundle.
    /// </summary>
    public void DisableRecursion();

    /// <summary>
    /// Gets the original parameters the asset load call was called with.
    /// </summary>
    public ResourceLoadParameters Parameters { get; }

    /// <summary>
    /// Gets the loaded assets. Override individual indices to change the asset reference that will be loaded.
    ///
    /// Consider using this if the load type is 'LoadByType' and you subscribed with 'OneCallbackPerLoadCall'.
    /// </summary>
    public UnityEngine.Object[] Assets { get; set; }

    /// <summary>
    /// Gets the loaded asset. This is simply equal to the first index of the Assets property, with some
    /// additional null guards to prevent NullReferenceExceptions when using it.
    /// </summary>
    public UnityEngine.Object Asset { get; set; }
}
```

The HookBehaviour is an enum with the following definition:

```C#
/// <summary>
/// Enum indicating how the resource redirector should treat the callback.
/// </summary>
public enum HookBehaviour
{
    /// <summary>
    /// Specifies that exactly one callback should be received per call to asset/resource load method.
    /// </summary>
    OneCallbackPerLoadCall = 1,

    /// <summary>
    /// Specifies that exactly one callback should be received per loaded asset/resources. This means
    /// that the 'Asset' property should be used over the 'Assets' property on the context object.
    /// Do note that when using this option, if no resources are returned by a load call, no callbacks
    /// will be received.
    /// </summary>
    OneCallbackPerResourceLoaded = 2
}
```

An important points to make here, is that there is both an `Asset` and an `Assets` property on the context object. These can be used interchangably, but an array will only ever be used if the following two conditions apply:
 * You've subscribed with `OneCallbackPerLoadCall`.
 * The `LoadType` in the `Parameters` property is `LoadByType`, which is the only type of resource loading that may return multiple resources.

In relation to this, it is worth mentioning that if a call to load assets returns 0 assets, you will not receive any callbacks if you subscribe through `OneCallbackPerResourceLoaded` where as if you subscribe through `OneCallbackPerLoadCall` you would still get your one callback.

If you update or replace the asset being loaded remember to call to `Complete` method to indicate your intentions regarding:
 * Whether the remaining postfixes should be called.

In addition, if we take a look at the `Parameters` property of the context object, we will find the following definition:

```C#
/// <summary>
/// Class representing the original parameters of the load call.
/// </summary>
public class ResourceLoadedParameters
{
    /// <summary>
    /// Gets the name of the resource being loaded. Will not be the complete resource path if 'LoadByType' is used.
    /// </summary>
    public string Path { get; set; }

    /// <summary>
    /// Gets the type that passed to the resource load call.
    /// </summary>
    public Type Type { get; set; }

    /// <summary>
    /// Gets the type of call that loaded this asset. If 'LoadByType' is specified
    /// multiple assets may be returned if subscribed as 'OneCallbackPerLoadCall'.
    /// </summary>
    public ResourceLoadType LoadType { get; }
}

/// <summary>
/// Enum representing the different ways a resource may be loaded.
/// </summary>
public enum ResourceLoadType
{
    /// <summary>
    /// Indicates that this call is loading all assets of a specific type (below a specific path) in the Resources API.
    /// </summary>
    LoadByType,

    /// <summary>
    /// Indicates that this call is loading a single named asset in the Resources API.
    /// </summary>
    LoadNamed,

    /// <summary>
    /// Indicates that this call is loading a single named built-in asset in the Resources API.
    /// </summary>
    LoadNamedBuiltIn
}
```

It is also worth mentioning that these hooks handles both synchronous and asynchronous loading of resources.

Hooks subscribed through hook behaviour `OneCallbackPerLoadCall` will be called before hooks with the behaviour `OneCallbackPerResourceLoaded`.

### AssetBundle Load Methods
It is also possible to hook the loading of `AssetBundles` themselves. Only prefix hooks are supported when loading an asset bundle.

The event callback chain looks like this `[AssetBundleLoading/AsyncAssetBundleLoading hooks] => [Original Method]`.

#### AssetBundle Synchrous Load Methods
The method `RegisterAssetBundleLoadingHook( Action<AssetBundleLoadingContext> action )` is used to hook the synchronous AssetBundle load methods.

This API is a prefix to the `AssetBundle` API, which means that it is called before the AssetBundle is loaded.

The `AssetBundleLoadingContext` class has the following definition:

```C#
/// <summary>
/// The operation context surrounding the AssetBundleLoading hook (synchronous).
/// </summary>
public class AssetBundleLoadingContext : IAssetBundleLoadingContext
{
    /// <summary>
    /// Gets a normalized path to the asset bundle that is:
    ///  * Relative to the current directory
    ///  * Lower-casing
    ///  * Uses '\' as separators.
    /// </summary>
    /// <returns></returns>
    public string GetNormalizedPath();

    /// <summary>
    /// Indicate your work is done and if any other hooks to this asset bundle load should be called.
    /// </summary>
    /// <param name="skipRemainingPrefixes">Indicate if the remaining prefixes should be skipped.</param>
    /// <param name="skipOriginalCall">Indicate if the original call should be skipped. If you set the asset bundle, you likely want to set this to true.</param>
    public void Complete( bool skipRemainingPrefixes = true, bool? skipOriginalCall = true );

    /// <summary>
    /// Disables recursive calls if you make an asset/asset bundle load call
    /// from within your callback. If you want to prevent recursion this should
    /// be called before you load the asset/asset bundle.
    /// </summary>
    public void DisableRecursion();

    /// <summary>
    /// Gets the parameters of the call.
    /// </summary>
    public AssetBundleLoadingParameters Parameters { get; }

    /// <summary>
    /// Gets or sets the AssetBundle being loaded.
    /// </summary>
    public AssetBundle Bundle { get; set; }
}
```

Because this is a prefix API, the `Bundle` property will be null when the method is called and it is up to you to set it to a different value if you can handle the specified path.

If you update the `Bundle` property, remember to call the `Complete` to indicate your intentions regarding:
 * Whether or not the remaining prefixes should be skipped.
 * Whether or not the original method should be skipped.

In addition, if we take a look at the `Parameters` property of the context object, we will find the following definition:

```C#
/// <summary>
/// Class representing the original parameters of the load call.
/// </summary>
public class AssetBundleLoadingParameters
{
    /// <summary>
    /// Gets or sets the loaded path. Only relevant for 'LoadFromFile'.
    /// </summary>
    public string Path { get; }

    /// <summary>
    /// Gets or sets the crc. Only relevant for 'LoadFromFile'.
    /// </summary>
    public uint Crc { get; }

    /// <summary>
    /// Gets or sets the offset. Only relevant for 'LoadFromFile'.
    /// </summary>
    public ulong Offset { get; }

    /// <summary>
    /// Gets the type of call that is loading this asset bundle.
    /// </summary>
    public AssetBundleLoadType LoadType { get; }
}

/// <summary>
/// Enum representing the different ways an asset bundle may be loaded.
/// </summary>
public enum AssetBundleLoadType
{
    /// <summary>
    /// Indicates that the asset bundle is being loaded through a call to 'LoadFromFile' or 'LoadFromFileAsync'.
    /// </summary>
    LoadFromFile,
}
```

As can be seen, the current implementation only hooks the LoadFromFile/LoadFromFileAsync ways of loading AssetBundles, but this may be expanded in the future.

It may also be worth looking at the `GetNormalizedPath()` method instead of the `Path` property of the original call parameters. This is because the path passed to the method can take literally any form:
 * Absolute path
 * Relative path
 * Include a stray '..' in the middle of the path
 
Another way to change the result of the asset bundle load operation is to change the value of the `Path`, `Crc` and `Offset` properties in the `Parameters` property. If you do this, you likely will not want to call the Complete method, as you will want the original method to still be called.

#### AssetBundle Asynchrounous Load Methods
The method `RegisterAsyncAssetBundleLoadingHook( Action<AsyncAssetBundleLoadingContext> action )` is used to hook the asynchronous AssetBundle load methods.

This API is a prefix to the `AssetBundle` API, which means that it is called before the `AssetBundleCreateRequest` is created.

The `AsyncAssetBundleLoadingContext` class has the following definition:

```C#
/// <summary>
/// The operation context surrounding the AsyncAssetBundleLoading hook (asynchronous).
/// </summary>
public class AsyncAssetBundleLoadingContext : IAssetBundleLoadingContext
{
    /// <summary>
    /// Gets a normalized path to the asset bundle that is:
    ///  * Relative to the current directory
    ///  * Lower-casing
    ///  * Uses '\' as separators.
    /// </summary>
    /// <returns></returns>
    public string GetNormalizedPath();

    /// <summary>
    /// Indicate your work is done and if any other hooks to this asset bundle load should be called.
    /// </summary>
    /// <param name="skipRemainingPrefixes">Indicate if the remaining prefixes should be skipped.</param>
    /// <param name="skipOriginalCall">Indicate if the original call should be skipped. If you set the request, you likely want to set this to true.</param>
    public void Complete( bool skipRemainingPrefixes = true, bool? skipOriginalCall = true );

    /// <summary>
    /// Disables recursive calls if you make an asset/asset bundle load call
    /// from within your callback. If you want to prevent recursion this should
    /// be called before you load the asset/asset bundle.
    /// </summary>
    public void DisableRecursion();

    /// <summary>
    /// Gets the parameters of the call.
    /// </summary>
    public AssetBundleLoadingParameters Parameters { get; }

    /// <summary>
    /// Gets or sets the AssetBundleCreateRequest being used to load the AssetBundle.
    /// </summary>
    public AssetBundleCreateRequest Request { get; set; }

    /// <summary>
    /// Gets or sets the AssetBundle being loaded.
    /// </summary>
    public AssetBundle Bundle { get; set; }

    /// <summary>
    /// Gets or sets how this load operation should be resolved.
    /// Setting the Bundle/Request property will automatically update this value.
    /// </summary>
    public AsyncAssetBundleLoadingResolve ResolveType { get; set; }
}
```

Because this is a prefix API, the `Request` property will be null when the method is called and it is up to you to set it to a different value if you can handle the specified path.

As you can see there is actually also a `Bundle` property available on the context object. Under normal circumstances, however, you cannot use the `Bundle` property on the the `AsyncAssetBundleLoadingContext`. In order to be able to use these, you must first call `ResourceRedirection.EnableSyncOverAsyncAssetLoads` once during your initialization logic. This will allow you to set the bundle directly so you don't have to go through the standard `AssetBundle` API to obtain a request object.

It is, however, recommended that if you can that you set the `Request` property instead of the `Bundle` property as that will keep the operation asynchronous and not block the game while the asset is being loaded.

If you update the `Request` property, remember to call the `Complete` to indicate your intentions regarding:
 * Whether or not the remaining prefixes should be skipped.
 * Whether or not the original method should be skipped.

In addition, if we take a look at the `Parameters` property of the context object, we will find the following definition:

```C#
/// <summary>
/// Class representing the original parameters of the load call.
/// </summary>
public class AssetBundleLoadingParameters
{
    /// <summary>
    /// Gets the loaded path. Only relevant for 'LoadFromFile'.
    /// </summary>
    public string Path { get; }

    /// <summary>
    /// Gets the crc. Only relevant for 'LoadFromFile'.
    /// </summary>
    public uint Crc { get; }

    /// <summary>
    /// Gets the offset. Only relevant for 'LoadFromFile'.
    /// </summary>
    public ulong Offset { get; }

    /// <summary>
    /// Gets the type of call that is loading this asset bundle.
    /// </summary>
    public AssetBundleLoadType LoadType { get; }
}

/// <summary>
/// Enum representing the different ways an asset bundle may be loaded.
/// </summary>
public enum AssetBundleLoadType
{
    /// <summary>
    /// Indicates that the asset bundle is being loaded through a call to 'LoadFromFile' or 'LoadFromFileAsync'.
    /// </summary>
    LoadFromFile,
}
```

As can be see, the current implementation only hooks the LoadFromFile/LoadFromFileAsync ways of loading AssetBundles, but this may be expanded in the future.

It may also be worth looking at the `GetNormalizedPath()` method instead of the `Path` property of the original call parameters. This is because the path passed to the method can take literally any form:
 * Absolute path
 * Relative path
 * Include a stray '..' in the middle of the path
 
Another way to change the result of the asset bundle load operation is to change the value of the `Path`, `Crc` and `Offset` properties in the `Parameters` property. If you do this, you likely will not want to call the Complete method, as you will want the original method to still be called.

An important additional way to subscribe to the prefix asset bundle loading operations are through the method `RegisterAsyncAndSyncAssetBundleLoadingHook( int priority, Action<IAssetBundleLoadingContext> action )`. This method will handle both async and sync asset bundle loading operations. The `IAssetLoadingContext` is an interface implemented by both the `AssetBundleLoadingContext` and `AsyncAssetBundleLoadingContext`.

Do note, that if you want to use this method you must first call the method `EnableSyncOverAsyncAssetLoads()` to enable the hooks required for this to work.

### About Recursion
As you may have noticed, all of the context classes shown in the previous sections had a method called `DisableRecursion` and that there is a method called `DisableRecursionPermanently` directly on the `ResourceRedirection` class.

The purpose of these method is, as it name states, to disable recursion. That only leaves the question, when does recursion occur?

Recursion will happen anytime you try to load an asset/resource/asset bundle from within your callback using the `AssetBundle` or `Resources` API. Essentially, what it means is that all callbacks (except the one loading the resource) will get a chance to modify the resource that is being loaded by your callback.

This may not always be desirable, so if you call the method `DisableRecursion` *before* you load your resource, this recursive behaviour is disabled. In many other cases, this behaviour is very desirable because it means that it is less important to set the *correct* priority, whatever that may be.

Recursion has an important side effect for other prefix/postfix callbacks, and that is that they will always be called if you make a recursive load call in your callback, even if you indicate through the `Complete` method that they should not be called. So if, in your scenario, it is important to avoid this, you must disable recursion.

`DisableRecursionPermanently` disables recursion permanently for all subscribers to the ResourceRedirection API no matter who calls it. This is a game-wide setting that should be decided upon between plugin developers rather than by the hand of the individual.

These options exists only because it is not currently known whether or not having recursion enabled gives the best experience to plugin developers.

### Implementing an Asset Redirector
Here's an example of how a resource redirection may be implemented to hook all `Texture2D` objects loaded through the `AssetBundle` API:

```C#
class TextureReplacementPlugin
{
    void Awake()
    {
        ResourceRedirection.RegisterAssetLoadedHook(
            behaviour: HookBehaviour.OneCallbackPerResourceLoaded,
            priority: 0,
            action: AssetLoaded );
    }

    public void AssetLoaded( AssetLoadedContext context )
    {
        if( context.Asset is Texture2D texture2d ) // also acts as a null check
        {
            // TODO: Modify, replace or dump the texture
                
            context.Asset = texture2d; // only need to update the reference if you created a new texture
            context.Complete(
                skipRemainingPostfixes: true );
        }
    }
}
```

### Implemting an AssetBundle Redirector
Here's an example of how a resource redirection may be implemented to redirect non-existing resources to a seperate 'mods' directory.

```C#
class AssetBundleRedirectorPlugin
{
    void Awake()
    {
        ResourceRedirection.RegisterAssetBundleLoadingHook(
            priority: 1000,
            action: AssetBundleLoading );

        ResourceRedirection.RegisterAsyncAssetBundleLoadingHook(
            priority: 1000,
            action: AsyncAssetBundleLoading );
    }

    public void AssetBundleLoading( AssetBundleLoadingContext context )
    {
        if( !File.Exists( context.Parameters.Path ) )
        {
            // the game is trying to load a path that does not exist, lets redirect to our own resources
		    
            // obtain different resource path
            var normalizedPath = context.GetNormalizedPath();
            var modFolderPath = Path.Combine( "mods", normalizedPath );
		    
            // if the path exists, let's load that instead
            if( File.Exists( modFolderPath ) )
            {
                var bundle = AssetBundle.LoadFromFile( modFolderPath );
		    
                context.Bundle = bundle;
                context.Complete(
                    skipRemainingPrefixes: true,
                    skipOriginalCall: true );
            }
        }
    }

    public void AsyncAssetBundleLoading( AsyncAssetBundleLoadingContext context )
    {
        if( !File.Exists( context.Parameters.Path ) )
            {
            // the game is trying to load a path that does not exist, lets redirect to our own resources
		    
            // obtain different resource path
            var normalizedPath = context.GetNormalizedPath();
            var modFolderPath = Path.Combine( "mods", normalizedPath );
		    
            // if the path exists, let's load that instead
            if( File.Exists( modFolderPath ) )
            {
                var request = AssetBundle.LoadFromFileAsync( modFolderPath );
		    
                context.Request = request;
                context.Complete(
                    skipRemainingPrefixes: true,
                    skipOriginalCall: true );
            }
        }
    }
}
```

Here's a smart way to implement the same thing, by having a single method that hooks both the synchronous and asynchronous method at the same time:

```C#
class AssetBundleRedirectorSyncOverAsyncPlugin
{
    void Awake()
    {
        ResourceRedirection.EnableSyncOverAsyncAssetLoads();
	    
        ResourceRedirection.RegisterAsyncAndSyncAssetBundleLoadingHook(
            priority: 1000,
            action: AssetBundleLoading );
    }

    public void AssetBundleLoading( IAssetBundleLoadingContext context )
    {
        if( !File.Exists( context.Parameters.Path ) )
        {
            // the game is trying to load a path that does not exist, lets redirect to our own resources
	    
            // obtain different resource path
            var normalizedPath = context.GetNormalizedPath();
            var modFolderPath = Path.Combine( "mods", normalizedPath );
	    
            // if the path exists, let's load that instead
            if( File.Exists( modFolderPath ) )
            {
                var bundle = AssetBundle.LoadFromFile( modFolderPath );
	    
                context.Bundle = bundle;
                context.Complete(
                    skipRemainingPrefixes: true,
                    skipOriginalCall: true );
            }
        }
    }
}
```

While this is clean it causes all asset bundles to be loaded synchronously, potentially locking up the game causing FPS lag. Another approach that also handles that could look like this:

```C#
class SmartAssetBundleRedirectorSyncOverAsyncPlugin
{
    void Awake()
    {
        ResourceRedirection.EnableSyncOverAsyncAssetLoads();
	    
        ResourceRedirection.RegisterAsyncAndSyncAssetBundleLoadingHook(
            priority: 1000,
            action: AssetBundleLoading );
    }

    public void AssetBundleLoading( IAssetBundleLoadingContext context )
    {
        if( !File.Exists( context.Parameters.Path ) )
        {
            // the game is trying to load a path that does not exist, lets redirect to our own resources
	    
            // obtain different resource path
            var normalizedPath = context.GetNormalizedPath();
            var modFolderPath = Path.Combine( "mods", normalizedPath );
	    
            // if the path exists, let's load that instead
            if( File.Exists( modFolderPath ) )
            {
                if( context is AsyncAssetBundleLoadingContext asyncContext )
                {
                    var request = AssetBundle.LoadFromFileAsync( modFolderPath );
                    asyncContext.Request = request;
                }
                else
                {
                    var bundle = AssetBundle.LoadFromFile( modFolderPath );
                    context.Bundle = bundle;
                }
	    
                context.Complete(
                    skipRemainingPrefixes: true,
                    skipOriginalCall: true );
            }
        }
    }
}
```

Here's the redirector that is activated if the `EmulateAssetBundles` option is enabled in the plugin, which allows loading asset bundles from a different location than the game is requesting the bundle from:

```C#
/// <summary>
/// Creates an asset bundle hook that attempts to load asset bundles in the emulation directory
/// over the default asset bundles if they exist.
/// </summary>
/// <param name="hookPriority">Priority of the hook.</param>
/// <param name="emulationDirectory">The directory to look for the asset bundles in.</param>
public static void EnableEmulateAssetBundles( int hookPriority, string emulationDirectory )
{
    RegisterAssetBundleLoadingHook( hookPriority, ctx => HandleAssetBundleEmulation( ctx, SetBundle ) );
    RegisterAsyncAssetBundleLoadingHook( hookPriority, ctx => HandleAssetBundleEmulation( ctx, SetRequest ) );
		    
    // define base callback
    void HandleAssetBundleEmulation<T>( T context, Action<T, string> changeBundle )
        where T : IAssetBundleLoadingContext
    {
        if( context.Parameters.LoadType == AssetBundleLoadType.LoadFromFile )
        {
            var normalizedPath = context.GetNormalizedPath();
            var emulatedPath = Path.Combine( emulationDirectory, normalizedPath );
            if( File.Exists( emulatedPath ) )
            {
                changeBundle( context, emulatedPath );
		    
                context.Complete(
                    skipRemainingPrefixes: true,
                    skipOriginalCall: true );
            }
        }
    }
		    
    // synchronous specific code
    void SetBundle( AssetBundleLoadingContext context, string path )
    {
        context.Bundle = AssetBundle.LoadFromFile( path, context.Parameters.Crc, context.Parameters.Offset );
    }
		    
    // asynchronous specific code
    void SetRequest( AsyncAssetBundleLoadingContext context, string path )
    {
        context.Request = AssetBundle.LoadFromFileAsync( path, context.Parameters.Crc, context.Parameters.Offset );
    }
}
```

Here's the redirector that is activated if the `RedirectMissingAssetBundles` option is enabled in the plugin, which essentially simply loads an empty asset bundle if an asset bundle cannot be found:

```C#
/// <summary>
/// Creates an asset bundle hook that redirects asset bundles loads to an empty
/// asset bundle if the file that is being loaded does not exist.
/// </summary>
/// <param name="hookPriority">Priority of the hook.</param>
public static void EnableRedirectMissingAssetBundlesToEmptyAssetBundle( int hookPriority )
{
    RegisterAssetBundleLoadingHook( hookPriority, ctx => HandleMissingBundle( ctx, SetBundle ) );
    RegisterAsyncAssetBundleLoadingHook( hookPriority, ctx => HandleMissingBundle( ctx, SetRequest ) );
	    
    // define base callback
    void HandleMissingBundle<TContext>( TContext context, Action<TContext, byte[]> changeBundle )
        where TContext : IAssetBundleLoadingContext
    {
        if( context.Parameters.LoadType == AssetBundleLoadType.LoadFromFile
            && !File.Exists( context.Parameters.Path ) )
        {
            var buffer = Properties.Resources.empty;
            CabHelper.RandomizeCab( buffer );
	    
            changeBundle( context, buffer );
	    
            context.Complete(
                skipRemainingPrefixes: true,
                skipOriginalCall: true );
	    
            XuaLogger.ResourceRedirector.Warn( "Tried to load non-existing asset bundle: " + context.Parameters.Path );
        }
    }
	    
    // synchronous specific code
    void SetBundle( AssetBundleLoadingContext context, byte[] assetBundleData )
    {
        var bundle = AssetBundle.LoadFromMemory( assetBundleData );
        context.Bundle = bundle;
    }
	    
    // asynchronous specific code
    void SetRequest( AsyncAssetBundleLoadingContext context, byte[] assetBundleData )
    {
        var request = AssetBundle.LoadFromMemoryAsync( assetBundleData );
        context.Request = request;
    }
}
```

### Implementing an Asset/Resource Handler (Auto Translator)
This section shows how to implement an asset/resource redirector that respects the Auto Translator configuration.

The `XUnity.AutoTranslator.Core.Plugin.dll` assembly has a base class that can be used to implement a plugin that dumps resources for the purposes of translation.

This class simply hooks the postfix to the load of assets from the `AssetBundle` and the `Resources` API. Here's how the base class looks:

```C#
/// <summary>
/// Base implementation of resource redirect handler that takes care of the plumming for a
/// resource redirector that is interested in either updating or dumping redirected resources.
/// </summary>
/// <typeparam name="TAsset">The type of asset being redirected.</typeparam>
public abstract class AssetLoadedHandlerBaseV2<TAsset>
    where TAsset : UnityEngine.Object
{
    /// <summary>
    /// Method invoked when an asset should be updated or replaced.
    /// </summary>
    /// <param name="calculatedModificationPath">This is the modification path calculated in the CalculateModificationFilePath method.</param>
    /// <param name="asset">The asset to be updated or replaced.</param>
    /// <param name="context">This is the context containing all relevant information for the resource redirection event.</param>
    /// <returns>A bool indicating if the event should be considered handled.</returns>
    protected abstract bool ReplaceOrUpdateAsset( string calculatedModificationPath, ref TAsset asset, IAssetOrResourceLoadedContext context );

    /// <summary>
    /// Method invoked when an asset should be dumped.
    /// </summary>
    /// <param name="calculatedModificationPath">This is the modification path calculated in the CalculateModificationFilePath method.</param>
    /// <param name="asset">The asset to be updated or replaced.</param>
    /// <param name="context">This is the context containing all relevant information for the resource redirection event.</param>
    /// <returns>A bool indicating if the event should be considered handled.</returns>
    protected abstract bool DumpAsset( string calculatedModificationPath, TAsset asset, IAssetOrResourceLoadedContext context );

    /// <summary>
    /// Method invoked when a new resource event is fired to calculate a unique path for the resource.
    /// </summary>
    /// <param name="asset">The asset to be updated or replaced.</param>
    /// <param name="context">This is the context containing all relevant information for the resource redirection event.</param>
    /// <returns>A string uniquely representing a path for the redirected resource.</returns>
    protected abstract string CalculateModificationFilePath( TAsset asset, IAssetOrResourceLoadedContext context );

    /// <summary>
    /// Method to be invoked to indicate if the asset should be handled or not.
    /// </summary>
    /// <param name="asset">The asset to be updated or replaced.</param>
    /// <param name="context">This is the context containing all relevant information for the resource redirection event.</param>
    /// <returns>A bool indicating if the asset should be handled.</returns>
    protected abstract bool ShouldHandleAsset( TAsset asset, IAssetOrResourceLoadedContext context );
}
```

The Auto Translation includes one default implementation of this class for TextAssets. It looks like this:

```C#
internal class TextAssetLoadedHandler : AssetLoadedHandlerBaseV2<TextAsset>
{
    protected override string CalculateModificationFilePath( TextAsset asset, IAssetOrResourceLoadedContext context )
    {
        return context.GetPreferredFilePath( asset, ".txt" );
    }

    protected override bool DumpAsset( string calculatedModificationPath, TextAsset asset, IAssetOrResourceLoadedContext context )
    {
        Directory.CreateDirectory( new FileInfo( calculatedModificationPath ).Directory.FullName );
        File.WriteAllBytes( calculatedModificationPath, asset.bytes );

        return true;
    }

    protected override bool ReplaceOrUpdateAsset( string calculatedModificationPath, ref TextAsset asset, IAssetOrResourceLoadedContext context )
    {
        RedirectedResource file;

        var files = RedirectedDirectory.GetFile( calculatedModificationPath ).ToList();
        if( files.Count == 0 )
        {
		    return false;
        }
        else
        {
            if( files.Count > 1 )
            {
                XuaLogger.AutoTranslator.Warn( "Found more than one resource file in the same path: " + calculatedModificationPath );
            }
		    
            file = files.FirstOrDefault();
        }

        if( file != null )
        {
            using( var stream = file.OpenStream() )
            {
                var data = stream.ReadFully( (int)stream.Length );
                var text = Encoding.UTF8.GetString( data );
		    
                var ext = asset.GetOrCreateExtensionData<TextAssetExtensionData>();
                ext.Data = data;
                ext.Text = text;
		    
                return true;
            }
        }
        return false;
    }

    protected override bool ShouldHandleAsset( TextAsset asset, IAssetOrResourceLoadedContext context )
    {
        return !context.HasReferenceBeenRedirectedBefore( asset );
    }
}
```

Note that when accessing the resource file, we do not use the standard file API to obtain a stream to get the data in the file. Instead we use the RedirectedDirectory facade. This will also look in ZIP files and simply treat a ZIP file as a directory when making the lookup.

Another examples of an implementation of this class would be for Koikatsu that enables replacing its custom resources:

```C#
public class ScenarioDataResourceRedirector : AssetLoadedHandlerBaseV2<ScenarioData>
{
   public ScenarioDataResourceRedirector()
   {
      CheckDirectory = true;
   }

   protected override string CalculateModificationFilePath( ScenarioData asset, IAssetOrResourceLoadedContext context )
   {
      return context.GetPreferredFilePathWithCustomFileName( asset, null )
         .Replace( ".unity3d", "" );
   }

   protected override bool DumpAsset( string calculatedModificationPath, ScenarioData asset, IAssetOrResourceLoadedContext context )
   {
      var defaultTranslationFile = Path.Combine( calculatedModificationPath, "translation.txt" );
      var cache = new SimpleTextTranslationCache(
         file: defaultTranslationFile,
         loadTranslationsInFile: false );

      foreach( var param in asset.list )
      {
         if( param.Command == Command.Text )
         {
            for( int i = 0; i < param.Args.Length; i++ )
            {
               var key = param.Args[ i ];

               if( !string.IsNullOrEmpty( key ) && LanguageHelper.IsTranslatable( key ) )
               {
                  cache.AddTranslationToCache( key, key );
               }
            }
         }
      }

      return true;
   }

   protected override bool ReplaceOrUpdateAsset( string calculatedModificationPath, ref ScenarioData asset, IAssetOrResourceLoadedContext context )
   {
      var defaultTranslationFile = Path.Combine( calculatedModificationPath, "translation.txt" );
      var redirectedResources = RedirectedDirectory.GetFilesInDirectory( calculatedModificationPath, ".txt" );
      var streams = redirectedResources.Select( x => x.OpenStream() );
      var cache = new SimpleTextTranslationCache(
         outputFile: defaultTranslationFile,
         inputStreams: streams,
         allowTranslationOverride: false,
         closeStreams: true );

      foreach( var param in asset.list )
      {
         if( param.Command == Command.Text )
         {
            for( int i = 0; i < param.Args.Length; i++ )
            {
               var key = param.Args[ i ];
               if( !string.IsNullOrEmpty( key ) )
               {
                  if( cache.TryGetTranslation( key, true, out var translated ) )
                  {
                     param.Args[ i ] = translated;
                  }
                  else if( AutoTranslatorSettings.IsDumpingRedirectedResourcesEnabled && LanguageHelper.IsTranslatable( key ) )
                  {
                     cache.AddTranslationToCache( key, key );
                  }
               }
            }
         }
      }

      return true;
   }

   protected override bool ShouldHandleAsset( ScenarioData asset, IAssetOrResourceLoadedContext context )
   {
      return !context.HasReferenceBeenRedirectedBefore( asset );
   }
}
```

Note that this implementation uses a `SimpleTextTranslationCache` to lookup translations. Using this class for translation lookups have the following benefits:
 * Whitespace doesn't have to match exactly.
 * It respects the `RedirectedResourceDetectionStrategy` configuration. If this is not respected the plugin may double translate certain texts.
 * When loading text translation files, it supports the same text format that is otherwise used by the plugin.

Once you have implemented one of these classes, you just need to instantiate it and it will do it's magic. 
