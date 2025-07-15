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

### 行为配置说明

#### 空白字符处理
本节描述了在执行翻译前后对空白字符处理产生影响的配置参数。**这些设置都不会影响在自动生成的翻译文件中放置的"未翻译文本"。**

对于自动翻译来说，正确的空白字符处理确实能够决定翻译的成败。控制空白字符处理的参数包括：
 * `IgnoreWhitespaceInDialogue`
 * `IgnoreWhitespaceInNGUI`
 * `MinDialogueChars`
 * `ForceSplitTextAfterCharacters`

插件首先确定是否应该执行特殊的空白字符删除操作。它根据参数`IgnoreWhitespaceInDialogue`、`IgnoreWhitespaceInNGUI`和`MinDialogueChars`来决定是否执行此操作：
 * `IgnoreWhitespaceInDialogue`：如果文本长度超过`MinDialogueChars`，则删除空白字符。
 * `IgnoreWhitespaceInNGUI`：如果文本来自NGUI组件，则删除空白字符。

在文本被配置的服务翻译后，`ForceSplitTextAfterCharacters`用于确定插件是否应该在特定字符数后强制将结果拆分为多行。

这种处理方式能够决定翻译成败的主要原因确实在于是否在将源文本发送到端点之前删除了空白字符。大多数端点（如GoogleTranslate）会分别考虑多行文本，如果包含不必要的换行符，通常会导致糟糕的翻译。

#### 文本后处理/预处理
虽然正确的空白字符处理在确保更好的翻译方面很有帮助，但这并不总是足够的。

`PreprocessorsFile`允许定义在文本发送到翻译器之前对其进行修改的条目。

`PostprocessorsFile`允许定义在从翻译器接收文本后对已翻译文本进行修改的条目。

#### UI调整大小
在对文本组件执行翻译时，结果文本通常比原始文本更大。这通常意味着文本组件中没有足够的空间来显示结果。本节描述了通过更改文本组件的重要参数来解决这个问题的方法。

默认情况下，插件会尝试一些基本的自动调整大小行为，这些行为由以下参数控制：`EnableUIResizing`、`ResizeUILineSpacingScale`、`ForceUIResizing`、`OverrideFont`和`OverrideFontTextMeshPro`。
 * `EnableUIResizing`：执行翻译时调整组件大小。
 * `ForceUIResizing`：始终调整所有组件的大小，无论何时。
 * `ResizeUILineSpacingScale`：更改调整大小的组件的行间距。仅适用于UGUI。
 * `OverrideFont`：无论`EnableUIResizing`和`ForceUIResizing`如何，都更改所有文本组件的字体。仅适用于UGUI。
 * `OverrideFontTextMeshPro`：考虑使用`FallbackFontTextMeshPro`代替。无论`EnableUIResizing`和`ForceUIResizing`如何，都更改所有文本组件的字体。仅适用于TextMeshPro。此选项可以以两种不同的方式加载字体。如果指定的字符串表示游戏文件夹中的路径，则该文件将尝试作为资源包加载（需要Unity 2018或更高版本（或者为目标游戏专门构建的自定义资源包））。如果不是，它将尝试通过Resources API加载。TextMeshPro通常分发的默认资源包括：`Fonts & Materials/LiberationSans SDF`或`Fonts & Materials/ARIAL SDF`。
 * `FallbackFontTextMeshPro`：添加一个回退字体，TextMesh Pro可以在不支持特定字符的情况下使用。

关于更改TextMeshPro字体的附加说明：您可以在发布选项卡中下载一些为Unity 2018和2019预构建的资源包，但目前它们还没有经过充分测试。如果您想尝试它们，只需下载.zip文件夹并将其中一个字体资源放入游戏文件夹中。然后通过在配置文件中的`OverrideFontTextMeshPro`中写入文件名来配置它。

UI组件的调整大小不是指更改其尺寸，而是指组件如何处理溢出。插件更改溢出参数，使文本更有可能被显示。

配置`EnableUIResizing`和`ForceUIResizing`还控制是否启用手动UI调整大小行为。有关更多信息，请参阅[此部分](#ui-字体调整)。

#### 减少翻译请求
以下旨在减少发送到翻译端点的请求数量：
 * `EnableBatching`：在支持的端点上将多个翻译请求批处理为一个。
 * `UseStaticTranslations`：启用各种英语到日语术语的内部查找字典。
 * `MaxCharactersPerTranslation`：指定要翻译的文本的最大长度。任何超过此长度的文本都将被插件忽略。不能大于1000。**绝不要在此值大于400的情况下重新分发此模组**

#### 罗马字"翻译"
输出`Language`的可能值之一是'romaji'。如果您选择此作为语言，您会发现游戏经常在显示翻译时出现问题，因为字体不能理解所使用的特殊字符，例如[macron diacritic](https://en.wikipedia.org/wiki/Macron_(diacritic))。

为了解决这个问题，当选择'romaji'作为`Language`时，可以对翻译应用后处理。这是通过选项`RomajiPostProcessing`完成的。此选项是一个由';'分隔的值列表：
 * `RemoveAllDiacritics`：从翻译文本中删除所有变音符号
 * `ReplaceMacronWithCircumflex`：将macron变音符号替换为circumflex。
 * `RemoveApostrophes`：某些翻译器可能会在'n'字符后包含撇号。应用此选项会删除这些撇号。
 * `ReplaceWideCharacters`：将宽幅日语字符替换为标准ASCII字符
 * `ReplaceHtmlEntities`：将所有html实体替换为其未转义字符

这种类型的后处理也适用于普通翻译，但使用选项`TranslationPostProcessing`，它可以使用相同的值。

#### MonoMod钩子
MonoMod钩子是在运行时创建的钩子，但不通过Harmony依赖项。Harmony有两个主要问题，这些钩子试图解决：
 * Harmony无法钩取没有主体的方法。
 * Harmony无法钩取`netstandard2.0` API表面下的方法，Unity的后续版本可以在此表面下构建。

MonoMod解决了这两个问题。为了使用MonoMod钩子，插件必须可用库`MonoMod.RuntimeDetours.dll`、`MonoMod.Utils.dll`和`Mono.Cecil.dll`。这些是可选依赖项。

这些仅在以下包中可用：
 * `XUnity.AutoTranslator-BepInEx-{VERSION}.zip`（因为所有依赖项都随BepInEx 5.x分发）
 * `XUnity.AutoTranslator-IPA-{VERSION}.zip`（因为所有依赖项都包含在包中）
 * `XUnity.AutoTranslator-ReiPatcher-{VERSION}.zip`（因为所有依赖项都包含在包中）

它们不在BepInEx 4.x中分发，因为在各种游戏的模组包中可能存在冲突。

以下配置控制MonoMod钩子：
 * `ForceMonoModHooks`：强制插件使用MonoMod钩子而不是Harmony钩子。

如果不强制使用MonoMod钩子，它们只有在可用且给定方法由于上述两个原因之一无法通过Harmony钩取时才使用。

#### 其他选项
 * `TextGetterCompatibilityMode`：此模式使游戏认为显示的文本没有被翻译。如果游戏使用显示给用户的文本来确定要执行的逻辑，这是必需的。您可以轻松确定是否需要此选项，如果您可以看到在切换翻译关闭时功能正常工作（热键：ALT+T）。
 * `IgnoreTextStartingWith`：禁用对以此';'分隔设置中的值开头的任何文本的翻译。[默认值](https://www.charbase.com/180e-unicode-mongolian-vowel-separator)是一个不占用空间的不可见字符。
 * `CopyToClipboard`：将要翻译的文本复制到剪贴板以支持Translation Aggregator等工具。
 * `ClipboardDebounceTime`：钩取文本和将其复制到剪贴板之间的延迟。这是为了避免剪贴板垃圾邮件。如果在此期间出现多个文本，它们将被连接。
 * `EnableSilentMode`：指示插件不应该打印与翻译相关的成功消息。
 * `BlacklistedIMGUIPlugins`：如果IMGUI窗口程序集/类/方法名称包含此列表中的任何字符串（不区分大小写），则该UI将不会被翻译。需要MonoMod钩子。这是一个由';'分隔的列表。
 * `OutputUntranslatableText`：指示是否应该将插件认为不可翻译的文本输出到指定的OutputFile。启用此选项可能还会将大量垃圾输出到`OutputFile`中，在潜在的重新分发之前应该删除这些垃圾。**绝不要在启用此选项的情况下重新分发模组。**
 * `IgnoreVirtualTextSetterCallingRules`：指示在尝试设置文本组件的文本时应该忽略虚拟方法调用的规则。在某些情况下可能有助于设置顽固组件的文本。
 * `RedirectedResourceDetectionStrategy`：指示插件是否以及如何尝试识别重定向资源以防止重复翻译。可以是["None", "AppendMongolianVowelSeparator", "AppendMongolianVowelSeparatorAndRemoveAppended", "AppendMongolianVowelSeparatorAndRemoveAll"]
 * `OutputTooLongText`：指示插件是否应该输出超过'MaxCharactersPerTranslation'的文本而不翻译它

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

### 插件特定的手动翻译
通常您可能想要为其他不能自然翻译的插件提供翻译。如前一节所述，这在此插件中显然也是可能的。但是，如果您想要提供应该特定于该插件的翻译，因为这种翻译会与不同的插件/通用翻译冲突，该怎么办？

为了添加插件特定的翻译，只需在文本翻译`Directory`中创建一个`Plugins`目录。在此目录中，您可以为要提供插件特定翻译的每个插件创建一个新目录。目录的名称应该与dll名称相同，但不包含扩展名（.dll）。

在此目录中，您可以像平常一样创建翻译文件。此外，您可以在这些文件中添加以下指令：

```
#enable fallback
```

这将允许插件特定的翻译回退到插件提供的通用/自动翻译。将此指令放置在哪个翻译文件中并不重要，只需要添加一次。

作为插件作者，还可以将这些翻译文件嵌入到您的插件中，并通过以下API通过代码注册它们：

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

### 替换
还可以添加在创建翻译之前应用于找到的文本的替换。这通过`SubstitutionFile`控制，它使用与普通翻译文本文件相同的格式，尽管不支持正则表达式等功能。

这对于替换经常被错误翻译的名称等很有用。

使用替换时，找到的出现将在生成的翻译中参数化，如下所示：

```
私は{{A}}=I am {{A}}
```

或者，如果使用配置`GenerateStaticSubstitutionTranslations=True`，翻译将不会被参数化。

在创建手动翻译时，请像使用正则表达式一样谨慎使用此文件，因为它可能对性能产生影响。

*注意：如果要翻译的文本包含富文本，目前无法参数化。*

### 正则表达式用法
文本翻译文件也支持正则表达式。始终记住谨慎使用正则表达式并限制其范围以避免性能问题。

正则表达式可以通过两种不同的方式应用于翻译。以下两节描述了这两种方式：

#### 标准正则表达式翻译
标准正则表达式翻译只是在无法找到直接查找的情况下直接应用于可翻译文本的正则表达式。

```
r:"^シンプルリング ([0-9]+)$"=Simple Ring $1
```

这些由以'r:'开头的未翻译文本标识。

#### 分割器正则表达式
有时游戏喜欢在屏幕上显示文本之前将文本组合。这意味着有时很难知道要添加到翻译文件中的文本是什么，因为它以多种不同的方式出现。

本节通过应用正则表达式将要翻译的文本分割成单独的部分，然后尝试为指定的文本进行查找来探索解决方案。

例如，假设一个装饰品（Simple Ring）将用以下行翻译`シンプルリング=Simple Ring`。现在假设这在游戏的多个文本框中出现，如`01 シンプルリング`和`02 シンプルリング`。在翻译文件中提供标准正则表达式来处理这种情况是行不通的，因为您需要为每个装饰品提供正则表达式，这根本不会有性能。

但是，如果我们在尝试进行查找之前分割翻译，它将允许我们在文件中只有一个简单的翻译，如下所示：`シンプルリング=Simple Ring`。

只需在翻译文件中放置以下正则表达式：

```
sr:"^([0-9]{2}) ([\S\s]+)$"=$1 $2
```

这将把要翻译的文本分成两部分，分别翻译它们，然后将它们重新组合。

这些由以'sr:'开头的未翻译文本标识。

还值得注意的是，如果配置了，这种方法可以递归使用。这意味着它允许被正则表达式分割进行翻译的单个字符串流入另一个分割器正则表达式，依此类推。

除了按索引标识每个组之外，它们还可以按名称标识，这允许组完全是附加的。让我们看一个结合所有这些功能的例子：

```
sr:"^\[(?<stat>[\w\s]+)(?<num_i>[\+\-]{1}[0-9]+)?\](?<after>[\s\S]+)?$"="[${stat}${num_i}]${after}"
```

在这个例子中有3个命名组，其中两个是可选的（标准正则表达式语法）。替换模式通过用`${}`围绕名称来标识这些命名组。

如果标识符名称以`_i`结尾，则意味着该字符串将不会尝试翻译，而是按原样传递。一般来说，这并不是真正需要的，因为插件足够智能，可以确定某些内容是否应该翻译。

那么这个正则表达式会分割什么呢？它会分割这样的字符串：

```
[DEF+14][ATK+64][DEX+34][AGI]
```

组`(?<stat>[\w]+)(?<num_i>[\+\-]{1}[0-9]+)?`匹配`[]`内的文本。如您所见，有两个组。第一个是必需的，表示文本。第二个是可选的，表示后面的加号/减号和数字。

组`(?<after>[\s\S]+)`匹配后面的任何内容。因此，它会尝试像任何其他文本一样翻译该文本，这可能直接流回到此分割器正则表达式中。

#### 正则表达式后处理
使用配置选项`RegexPostProcessing`，还可以对正则表达式的组应用后处理。对于`sr:`正则表达式，它们只应用于标识符名称以`_i`结尾的组。

### UI字体调整
还可以手动控制文本组件的字体大小。这在翻译文本使用的空间比未翻译文本多时很有用。

您可以在放置在翻译`Directory`中以`resizer.txt`结尾的文件中控制这一点。此文件采用简单的语法，如下所示：

```
CharaCustom/CustomControl/CanvasDraw=ChangeFontSizeByPercentage(0.5)
```

在这些文件中，等号的左侧表示必须调整字体大小的组件的（部分）路径。右侧表示要对这些文本执行的命令的';'分隔列表。

在显示的示例中，它将指定路径下所有文本的字体大小减少到50%。

像任何其他翻译文件一样，这些文件也支持翻译范围设定，如[此部分](#翻译范围设定)所述。

存在以下类型的命令：
 * 将字体大小更改为静态大小的命令：
   * `ChangeFontSizeByPercentage(double percentage)`：其中百分比是要减少到的原始字体大小的百分比。
   * `ChangeFontSize(int size)`：其中size是字体的新大小
   * `IgnoreFontSize()`：这可用于重置在非常"非特定"路径上设置的字体调整行为。
 * 控制自动调整大小的命令：
   * `AutoResize(bool enabled, minSize, maxSize)`：其中enabled控制是否应启用自动调整大小行为。最后两个参数是可选的。
     * minSize、maxSize可能的值：[keep, none, any number]
 * 控制行间距的命令（仅限UGUI）：
   * `UGUI_ChangeLineSpacingByPercentage(float percentage)`
   * `UGUI_ChangeLineSpacing(float lineSpacing)`
 * 控制水平溢出的命令（仅限UGUI）：
   * `UGUI_HorizontalOverflow(string mode)` - 可能的值：[wrap, overflow]
 * 控制垂直溢出的命令（仅限UGUI）：
   * `UGUI_VerticalOverflow(string mode)` - 可能的值：[truncate, overflow]
 * 控制溢出的命令（仅限TMP）：
   * `TMP_Overflow(string mode)` - [可能的值](https://docs.unity3d.com/Packages/com.unity.textmeshpro@3.0/api/TMPro.TextOverflowModes.html)
 * 控制文本对齐的命令（仅限TMP）：
   * `TMP_Alignment(string mode)` - [可能的值](https://docs.unity3d.com/Packages/com.unity.textmeshpro@3.0/api/TMPro.TextAlignmentOptions.html)

但是等等！我如何确定要使用的路径？此插件没有提供简单确定此路径的方法，但有其他插件允许您这样做。

有两种方法，您可能需要同时使用它们：
 * 使用[Runtime Unity Editor](https://github.com/ManlyMarco/RuntimeUnityEditor)来确定这些。
 * 启用选项`[Behaviour] EnableTextPathLogging=True`，它将记录所有文本更改的文本组件的路径。

### 翻译范围设定
当涉及将翻译范围限定为游戏的一部分时，有以下两个选项可用：

翻译文件支持以下指令：
 * `#set level 1,2,3` 告诉插件，此文件中此行之后的翻译只能在ID为1、2或3的场景中应用。
 * `#unset level 1,2,3` 告诉插件，此文件中此行之后的翻译不应在ID为1、2或3的场景中应用。如果未设置级别，则所有指定的翻译都是全局的。
 * `#set exe game1,game2` 告诉插件，此文件中此行之后的翻译只能在通过名为game1或game2的可执行文件运行游戏时应用。
 * `#unset exe game1,game2` 告诉插件，此文件中此行之后的翻译不应在通过名为game1或game2的可执行文件运行游戏时应用。如果未设置exe，则所有指定的翻译都是全局的。
 * `#set required-resolution height > 1280 && width > 720` 告诉插件，此文件中此行之后的翻译只有在分辨率大于指定时才应用。当前实现仅处理游戏启动时使用的分辨率。
 * `#unset required-resolution` 告诉插件忽略之前指定的`#set required-resolution`指令。

为了使其工作，以下配置选项必须为`True`：

```
[Behaviour]
EnableTranslationScoping=True
```

此外，此行为在`OutputFile`中不可用。

您始终可以通过使用热键CTRL+ALT+NP7来查看加载了哪些级别。

翻译范围设定的另一种方法是通过文件名。可以告诉插件在哪里查找翻译文件。可以使用变量{GameExeName}对这些路径进行参数化。

为每个可执行文件分离翻译的示例配置：

```
[Files]
Directory=Translation\{GameExeName}\{Lang}\Text
Directory=Translation\{GameExeName}\{Lang}\Text\_AutoGeneratedTranslations.txt
Directory=Translation\{GameExeName}\{Lang}\Text\_Substitutions.txt
```

那么什么时候应该使用翻译范围设定呢？这取决于范围类型：
 * `level`范围实际上只应用于避免翻译冲突
 * `exe`范围可用于避免翻译冲突和增强性能

### 文本查找和空白字符处理
本节旨在让翻译人员了解此插件如何查找文本并提供翻译。

在最简单的形式中，插件的工作方式是作为未翻译文本字符串的字典。当插件看到一个它认为未翻译的文本时，它会尝试在字典中查找该文本字符串，如果找到结果，它会显示找到的翻译。

然而，世界并不总是那么简单。根据游戏使用的引擎/文本框架，未翻译的文本字符串在不同上下文中使用时可能略有不同。例如，对于VN，出现在"ADV历史"视图中的文本字符串可能与最初显示给用户时的确切文本字符串不同。

**示例：**
```
「こう見えて怒っているんですよ？……失礼しますね」
「こう見えて怒っているんですよ？\n ……失礼しますね」
```

这些文本字符串不同，如果最终翻译应该相同，必须多次翻译相同的文本会很烦人。

实际上，只需要其中一个翻译。原因如下：（仍然非常简化）：
 1. 当插件看到未翻译的文本时，它实际上会进行四次查找，而不是一次。这些查找按顺序是：
    * 基于未触及的原始文本
    * 基于原始文本但没有前导/尾随空白字符。如果找到，前导/尾随空白字符会添加到结果翻译中
    * 基于原始文本但没有围绕换行的内部非重复空白字符
    * 基于原始文本但没有前导/尾随空白字符和围绕换行的内部非重复空白字符。如果找到，前导/尾随空白字符会添加到结果翻译中

这意味着对于以下字符串`\n 「こう見えて怒っているんですよ？\n ……失礼しますね」`，插件会进行以下查找：
```
\n 「こう見えて怒っているんですよ？\n ……失礼しますね」
「こう見えて怒っているんですよ？\n ……失礼しますね」
\n 「こう見えて怒っているんですよ？……失礼しますね」
「こう見えて怒っているんですよ？……失礼しますね」
```

 2. 当插件加载（手动/自动）翻译时，它不会创建一个字典条目，而是三个。这些是：
    * 基于未触及的原始文本和原始翻译
    * 基于原始文本（没有前导/尾随空白字符）和原始翻译（没有前导/尾随空白字符）
    * 基于原始文本（没有前导/尾随空白字符和围绕换行的内部非重复空白字符）和原始翻译（没有前导/尾随空白字符和围绕换行的内部非重复空白字符）
   
这意味着对于以下字符串`\n 「こう見えて怒っているんですよ？\n ……失礼しますね」`，插件会创建以下条目：
```
\n 「こう見えて怒っているんですよ？\n ……失礼しますね」
「こう見えて怒っているんですよ？\n ……失礼しますね」
「こう見えて怒っているんですよ？……失礼しますね」
```

这意味着您可以为这两种情况提供单个翻译。您认为哪种更好由您决定。

另一个需要注意的是，插件始终会在翻译文件中输出未修改的原始文本。但如果它之后看到与该文本字符串"兼容"的另一个文本（由于上述文本修改），默认情况下它不会输出这个新文本。

这由配置选项`CacheWhitespaceDifferences=False`控制。您可以将其更改为true，它会为每个唯一文本输出一个新条目，即使唯一的差异是空白字符。显然，实际出现在翻译文件中的翻译对始终优先于基于现有翻译对生成的翻译对。

*注意：与级别范围翻译相关的空白字符差异无论此设置如何都不会输出。*

### 资源重定向
有时，通过直接覆盖游戏资源文件来为游戏提供翻译更容易。但是，直接覆盖游戏资源文件也有问题，因为这意味着修改可能只适用于游戏的一个版本。

为了克服这个问题，并允许修改资源文件，此插件还有一个资源重定向器模块，允许重定向游戏加载的任何类型的资源。

在深入了解此模块的详细信息之前，值得一提的是：
 * 它不是一个插件。相反，它只是一个不依赖于任何插件管理器的库（它确实带有一个与插件兼容的BepInEx DLL，但这只是为了管理配置）。
 * 它是游戏独立的。
 * 虽然它可能与自动翻译器一起重新分发，但它完全独立于自动翻译器，可以在不安装自动翻译器的情况下使用。

资源重定向器工作所需的DLL是`XUnity.Common.dll`和`XUnity.ResourceRedirector.dll`。这些库本身不做任何事情。

默认情况下，自动翻译器插件带有一个用于`TextAsset`的资源重定向器，它基本上将原始文本资源输出到文件系统，允许它们被单独覆盖。

可以为特定游戏实现更多重定向器，尽管这需要编程知识，请参阅[此部分](#实现资源重定向器)以获取更多信息。

自动翻译器具有以下资源重定向器特定配置：
 * `PreferredStoragePath`：指示自动翻译器应该在哪里存储重定向的资源。
 * `EnableTextAssetRedirector`：指示是否启用TextAsset重定向器。
 * `LogAllLoadedResources`：指示资源重定向器是否应该将所有资源记录到控制台（也可以通过资源重定向器API界面控制）。
 * `EnableDumping`：指示是否应该转储重定向到自动翻译器的资源以便在可能的情况下覆盖。
 * `CacheMetadataForAllFiles`：当文件位于PreferredStoragePath中的ZIP文件中时，这些文件在内存中被索引以避免在加载时执行文件检查IO。启用此选项将对物理文件执行相同操作

放置在`PreferredStoragePath`中的ZIP文件将在启动时被索引，允许重定向的资源被压缩和打包。当文件放在zip文件中时，在文件查找期间zip文件被简单地视为不存在。

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

`TextureDirectory` 指定转储纹理和从中加载纹理的目录。加载也会从指定目录的所有子目录中进行，因此您可以将转储的图像移动到任何您希望的文件夹结构中。

`EnableTextureTranslation` 启用纹理翻译。这基本上意味着纹理将从`TextureDirectory`及其子目录加载。这些图像将替换游戏使用的游戏内图像。

`EnableTextureDumping` 启用纹理转储。这意味着模组将转储任何尚未转储到`TextureDirectory`的图像。转储纹理时，也可能值得启用`EnableTextureScanOnSceneLoad`以更快地找到所有需要翻译的纹理。**绝不要在启用此选项的情况下重新分发模组。**

`EnableTextureScanOnSceneLoad` 允许插件在场景加载事件上扫描纹理对象。这使插件能够在场景加载期间（通常是在加载画面等期间）以微小的性能成本找到更多纹理。但是，由于Unity的工作方式，并非所有这些都保证可以被替换。如果您发现转储的图像无法翻译，请报告。但是，请认识到此模组主要用于替换UI纹理，而不是3D网格的纹理。

`EnableSpriteRendererHooking` 允许插件尝试钩取SpriteRenderer。这是一个单独的选项，因为SpriteRenderer实际上无法正确钩取，实现的变通方法可能在某些情况下对性能产生理论影响。

`LoadUnmodifiedTextures` 启用插件是否应该加载未修改的纹理。这仅对调试有用，并且可能导致各种视觉故障，特别是如果还启用了`EnableTextureScanOnSceneLoad`。**绝不要在启用此选项的情况下重新分发模组。**

`EnableTextureToggling` 启用ALT+T热键是否也会切换纹理。这绝不保证能够工作，特别是如果还启用了`EnableTextureScanOnSceneLoad`。**绝不要在启用此选项的情况下重新分发模组。**

`DuplicateTextureNames` 指定游戏中在相同资源名称下使用的不同纹理。插件将回退到'FromImageData'来识别这些图像。

`DetectDuplicateTextureNames` 指定插件应该识别哪些图像名称是重复的，并自动使用这些名称更新配置。**绝不要在启用此选项的情况下重新分发模组。**

`EnableLegacyTextureLoading` 指定插件应该尝试以不同方式加载图像，如果Unity引擎较旧（验证版本低于5.3），这可能相关。除非加载的图像不是您期望的，否则不应使用此选项。

`CacheTexturesInMemory` 指定所有翻译纹理都应该保留在内存中以优化性能。可以禁用以减少内存使用。

`TextureHashGenerationStrategy` 指定如何识别图像。当图像被存储时，游戏需要某种方式将它们与必须替换的图像关联起来。
这是通过存储在每个图像文件名中方括号中的哈希值来完成的，如下所示：`file_name [0223B639A2-6E698E9272].png`。此配置指定如何生成这些哈希值：
 * `FromImageName` 意味着哈希是从游戏用于图像的内部资源名称生成的，这可能不存在于所有图像中，甚至可能不是唯一的。但是，它通常相当可靠。如果图像没有资源名称，它将不会被转储。
 * `FromImageData` 意味着哈希是从存储在图像中的数据生成的，这保证存在于所有图像中。但是，生成哈希会带来性能成本，最终用户也会承担这种成本。
 * `FromImageNameAndScene` 意味着它应该使用名称和场景来生成哈希。名称仍然是此功能工作所必需的。使用此选项时，相同的纹理可能以不同的哈希被转储，这是不可取的，但如果名称本身不唯一且`FromImageData`选项导致性能问题，某些游戏可能需要这样做。如果使用此选项，建议也启用`EnableTextureScanOnSceneLoad`。

在处理这些选项时，您需要注意一个重要的陷阱，即如果存在以下任何选项：`EnableTextureDumping=True`、`EnableTextureToggling=True`、`TextureHashGenerationStrategy=FromImageData`，那么游戏将需要从游戏中找到的所有图像读取原始数据以替换图像，这是一项昂贵的操作。

因此建议使用`TextureHashGenerationStrategy=FromImageName`。很可能，没有资源名称的图像无论如何都不会有翻译的兴趣。

如果您重新分发带有翻译图像的此模组，建议您删除所有您不打算翻译或根本未翻译的图像。

您还可以将文件名更改为您想要的任何名称，只要您保持哈希附加到文件名的末尾。

如果您从此部分中得到任何启发，应该是以下两点：
 * **绝不要在启用`EnableTextureDumping=True`、`EnableTextureToggling=True`、`LoadUnmodifiedTextures=True`或`DetectDuplicateTextureNames=true`的情况下重新分发模组**
 * **只有在游戏绝对需要的情况下才启用`TextureHashGenerationStrategy=FromImageData`重新分发模组。**

### 文件名中哈希生成的技术细节
生成的文件名中实际上有两个哈希，由短划线（-）分隔：
 * 第一个哈希是基于使用的`TextureHashGenerationStrategy`的SHA1（仅前5个字节）。如果指定了`FromImageName`，则基于UTF8（无BOM）表示。
 * 第二个哈希是基于图像中数据的SHA1（仅前5个字节）。这用于确定图像是否已被修改，因此未编辑的图像不会被加载。除非指定了`LoadUnmodifiedTextures`。

如果指定了`TextureHashGenerationStrategy=FromImageData`，每个文件名中只会出现一个哈希，因为该单个哈希可用于识别图像并确定是否已被编辑。

## 与自动翻译器集成
*注意：以下所有内容都需要编程知识！*

### 实现可以查询翻译的插件
作为模组作者，您可能想要从插件查询翻译。这很容易做到，看一下下面的示例。

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

### 实现自动翻译器不应干扰的组件
作为模组作者，您可能不希望自动翻译器干扰您的模组UI。如果是这种情况，有两种方法告诉自动翻译器不要执行任何翻译：
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

## 实现翻译器
从3.0.0版本开始，您现在也可以实现自己的翻译器。

为了做到这一点，您所要做的就是实现以下接口，构建程序集并将生成的DLL放在`Translators`文件夹中。

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

### 基于在线服务实现翻译器的重要说明
每当您基于在线服务实现翻译器时，重要的是不要以滥用的方式使用它。例如：
 * 建立大量连接
 * 执行网页抓取而不是使用可用的API
 * 向其发出并发请求
 * *如果服务未经过身份验证，这尤其重要*

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

### 实现方法
按照以下步骤：
 1. 从[发布页面](../../releases)下载XUnity.AutoTranslator-Developer-{VERSION}.zip
 2. 在Visual Studio 2017或更高版本中启动新项目（.NET 3.5）。我建议为您的程序集/项目使用与您将在接口实现中使用的"Id"相同的名称。这使用户更容易知道如何配置您的翻译器
    * 我建议使用"类库（.NET Standard）"，并简单地编辑生成的.csproj文件以使用'net35'而不是'netstandard2.0'。这生成更简洁的.csproj文件。
 3. 添加对您在第1步中下载的XUnity.AutoTranslator.Plugin.Core.dll的引用
 4. 您不需要直接引用UnityEngine.dll程序集。这很好，因为您不需要担心使用哪个版本的Unity。
    * 如果您确实需要对此程序集的引用（因为您需要其中的功能），请考虑使用它的旧版本（如果Managed文件夹中存在`UnityEngine.CoreModule.dll`，则它不是旧版本！）
 5. 创建一个新类，该类：
    * 实现`ITranslateEndpoint`接口
    * 继承自`HttpEndpoint`类
    * 继承自`WwwEndpoint`类
    * 继承自`ExtProtocolEndpoint`类

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

## 实现资源重定向器
资源重定向器允许您修改通过`Resources`和`AssetBundle` API加载的资源，在游戏加载它们时进行修改。

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

让我们为这个API添加一些注释。

### 资源加载/已加载方法
资源重定向器在从`AssetBundle` API加载资源时提供后缀和前缀回调。

事件回调链看起来像这样：`[AssetLoading / AsyncAssetLoading钩子] => [原始方法] => [AssetLoaded钩子]`。`AssetLoaded`事件处理同步和异步加载资源的后缀。

#### 资源加载方法
方法`RegisterAssetLoadingHook( HookBehaviour behaviour, Action<AssetLoadingContext> action )`和`RegisterAsyncAssetLoadingHook( int priority, Action<AsyncAssetLoadingContext> action )`在加载资源时钩入`AssetBundle` API。

这些方法注册前缀回调，这意味着在调用它们时资源本身尚未加载。

回调分别以`AssetLoadingContext`和`AsyncAssetLoadingContext`类型作为参数。让我们看看它们的定义：

```C#
/// <summary>
/// 围绕AssetLoading钩子的操作上下文（同步）。
/// </summary>
public class AssetLoadingContext : IAssetLoadingContext
{
    /// <summary>
    /// 获取加载资源包时使用的原始路径。
    /// </summary>
    /// <returns>加载资源包时使用的未修改的原始路径。</returns>
    public string GetAssetBundlePath();

    /// <summary>
    /// 获取资源包的规范化路径，该路径：
    ///  * 相对于当前目录
    ///  * 小写
    ///  * 使用'\'作为分隔符。
    /// </summary>
    /// <returns></returns>
    public string GetNormalizedAssetBundlePath();

    /// <summary>
    /// 指示您的工作已完成，以及是否应该调用对此资源/资源加载的任何其他钩子。
    /// </summary>
    /// <param name="skipRemainingPrefixes">指示是否应该跳过剩余的前缀。</param>
    /// <param name="skipOriginalCall">指示是否应该跳过原始调用。如果您设置了资源，您可能希望将其设置为true。</param>
    /// <param name="skipAllPostfixes">指示是否应该跳过后缀。</param>
    public void Complete( bool skipRemainingPrefixes = true, bool? skipOriginalCall = true, bool? skipAllPostfixes = true );

    /// <summary>
    /// 如果您在回调中进行资源/资源包加载调用，则禁用递归调用。
    /// 如果您想防止递归，应该在加载资源/资源包之前调用此方法。
    /// </summary>
    public void DisableRecursion();

    /// <summary>
    /// 获取调用资源加载调用时使用的原始参数。
    /// </summary>
    public AssetLoadingParameters Parameters { get; }

    /// <summary>
    /// 获取与加载资源关联的AssetBundle。
    /// </summary>
    public AssetBundle Bundle { get; }

    /// <summary>
    /// 获取或设置加载的资源。
    ///
    /// 如果加载类型是'LoadByType'或'LoadNamedWithSubAssets'，请考虑使用此属性。
    /// </summary>
    public UnityEngine.Object[] Assets { get; set; }

    /// <summary>
    /// 获取或设置加载的资源。这简单地等于Assets属性的第一个索引，并有一些
    /// 额外的null保护以防止在使用时出现NullReferenceExceptions。
    /// </summary>
    public UnityEngine.Object Asset { get; set; }
}

/// <summary>
/// 围绕AsyncAssetLoading钩子的操作上下文（异步）。
/// </summary>
public class AsyncAssetLoadingContext : IAssetLoadingContext
{
    /// <summary>
    /// 获取加载资源包时使用的原始路径。
    /// </summary>
    /// <returns>加载资源包时使用的未修改的原始路径。</returns>
    public string GetAssetBundlePath();

    /// <summary>
    /// 获取资源包的规范化路径，该路径：
    ///  * 相对于当前目录
    ///  * 小写
    ///  * 使用'\'作为分隔符。
    /// </summary>
    /// <returns></returns>
    public string GetNormalizedAssetBundlePath();

    /// <summary>
    /// 指示您的工作已完成，以及是否应该调用对此资源/资源加载的任何其他钩子。
    /// </summary>
    /// <param name="skipRemainingPrefixes">指示是否应该跳过剩余的前缀。</param>
    /// <param name="skipOriginalCall">指示是否应该跳过原始调用。如果您设置了资源，您可能希望将其设置为true。</param>
    /// <param name="skipAllPostfixes">指示是否应该跳过后缀。</param>
    public void Complete( bool skipRemainingPrefixes = true, bool? skipOriginalCall = true, bool? skipAllPostfixes = true );

    /// <summary>
    /// 如果您在回调中进行资源/资源包加载调用，则禁用递归调用。
    /// 如果您想防止递归，应该在加载资源/资源包之前调用此方法。
    /// </summary>
    public void DisableRecursion();

    /// <summary>
    /// 获取调用资源加载调用时使用的原始参数。
    /// </summary>
    public AssetLoadingParameters Parameters { get; }

    /// <summary>
    /// 获取与加载资源关联的AssetBundle。
    /// </summary>
    public AssetBundle Bundle { get; }

    /// <summary>
    /// 获取或设置用于加载资源的AssetBundleRequest。
    /// </summary>
    public AssetBundleRequest Request { get; set; }

    /// <summary>
    /// 获取或设置加载的资源。
    ///
    /// 如果加载类型是'LoadByType'或'LoadNamedWithSubAssets'，请考虑使用此属性。
    /// </summary>
    UnityEngine.Object[] Assets { get; set; }

    /// <summary>
    /// 获取或设置加载的资源。这简单地等于Assets属性的第一个索引，并有一些
    /// 额外的null保护以防止在使用时出现NullReferenceExceptions。
    /// </summary>
    UnityEngine.Object Asset { get; set; }

    /// <summary>
    /// 获取或设置此加载操作应如何解析。
    /// 设置Asset/Assets/Request属性将自动更新此值。
    /// </summary>
    public AsyncAssetLoadingResolve ResolveType { get; set; }
}
```

这两个上下文之间的唯一区别是，一个有您可以设置的`Asset/Assets`属性，而另一个有您可以设置的`Request`属性。

现在，如果您真的注意到了您正在阅读的内容（！？），您会注意到上述两个上下文对象都有一个可以设置的`Asset/Assets`属性。

但是，在正常情况下，您不能在`AsyncAssetLoadingContext`上使用`Assets/Asset`属性。为了能够使用这些属性，您必须首先在初始化逻辑中调用一次`ResourceRedirection.EnableSyncOverAsyncAssetLoads`。这将允许您直接设置资源，因此您不必通过标准的`AssetBundle` API来获取请求对象。

但是，建议如果可以的话，您应该设置`Request`属性而不是`Assets/Asset`属性，因为这将保持操作异步，并且在加载资源时不会阻塞游戏。

如果您可以处理资源的加载，请记住调用`Complete`方法来指示您的意图：
 * 是否应该跳过其余已注册的前缀。
 * 是否应该跳过原始方法。
 * 是否应该跳过所有后缀。

这里需要注意的重要一点是，上下文对象上既有`Asset`属性也有`Assets`属性。这些可以互换使用，但只有在以下条件适用时才会使用数组：
 * `Parameters`属性中的`LoadType`是`LoadByType`或`LoadNamedWithSubAssets`，这是唯一可能返回多个资源的资源加载类型。

最后，如果我们查看上下文对象的`Parameters`属性，我们会找到以下定义：

```C#
/// <summary>
/// 表示加载调用的原始参数的类。
/// </summary>
public class AssetLoadingParameters
{
    /// <summary>
    /// 获取或设置正在加载的资源的名称。如果通过'LoadMainAsset'或'LoadByType'加载，将为null。
    /// </summary>
    public string Name { get; set; }

    /// <summary>
    /// 获取或设置传递给资源加载调用的类型。
    /// </summary>
    public Type Type { get; set; }

    /// <summary>
    /// 获取加载此资源的调用类型。如果指定了'LoadByType'或'LoadNamedWithSubAssets'，
    /// 如果订阅为'OneCallbackPerLoadCall'，可能会返回多个资源。
    /// </summary>
    public AssetLoadType LoadType { get; }
}

/// <summary>
/// 表示资源可能被加载的不同方式的枚举。
/// </summary>
public enum AssetLoadType
{
    /// <summary>
    /// 指示此资源已在AssetBundle API中作为'mainAsset'加载。
    /// </summary>
    LoadMainAsset,

    /// <summary>
    /// 指示此调用正在AssetBundle API中加载特定类型的所有资源。
    /// </summary>
    LoadByType,

    /// <summary>
    /// 指示此调用正在AssetBundle API中加载特定命名的资源。
    /// </summary>
    LoadNamed,

    /// <summary>
    /// Indicates that this call is loading a specific named asset and all those below it in the AssetBundle API.
    /// </summary>
    LoadNamedWithSubAssets
}
```

更改资源加载操作结果的另一种方法是更改`Parameters`属性中的`Name`和`Type`属性的值。如果您这样做，您可能不希望调用Complete方法，因为您希望原始方法仍然被调用。

订阅前缀资源加载操作的另一个重要方法是通过方法`RegisterAsyncAndSyncAssetLoadingHook( int priority, Action<IAssetLoadingContext> action )`。此方法将处理异步和同步资源加载操作。`IAssetLoadingContext`是由`AssetLoadingContext`和`AsyncAssetLoadingContext`实现的接口。

请注意，如果您想使用此方法，您必须首先调用方法`EnableSyncOverAsyncAssetLoads()`来启用此功能所需的钩子。

#### 资源已加载方法
方法`RegisterAssetLoadedHook( HookBehaviour behaviour, Action<AssetLoadedContext> action )`钩入UnityEngine中的`AssetBundle` API。每当通过此API加载资源时，都会向这些钩子发送回调。

此API是`AssetBundle` API的后缀钩子，这意味着它在原始资源已经加载之后首次被调用，但仍可替换。

`AssetLoadedContext`类具有以下定义：

```C#
/// <summary>
/// 围绕AssetLoaded钩子的操作上下文。
/// </summary>
public class AssetLoadedContext : IAssetOrResourceLoadedContext
{
    /// <summary>
    /// 获取一个布尔值，指示此资源是否已经被重定向过。
    /// </summary>
    public bool HasReferenceBeenRedirectedBefore( UnityEngine.Object asset );

    /// <summary>
    /// 获取特定资源的文件系统路径，该路径应该是唯一的。
    /// </summary>
    /// <param name="asset"></param>
    /// <returns></returns>
    public string GetUniqueFileSystemAssetPath( UnityEngine.Object asset );

    /// <summary>
    /// 获取加载资源包时使用的原始路径。
    /// </summary>
    /// <returns>加载资源包时使用的未修改的原始路径。</returns>
    public string GetAssetBundlePath();

    /// <summary>
    /// 获取资源包的规范化路径，该路径：
    ///  * 相对于当前目录
    ///  * 小写
    ///  * 使用'\'作为分隔符。
    /// </summary>
    /// <returns></returns>
    public string GetNormalizedAssetBundlePath();

    /// <summary>
    /// 指示您的工作已完成，以及是否应该调用对此资源/资源加载的任何其他钩子。
    /// </summary>
    /// <param name="skipRemainingPostfixes">指示是否应该跳过任何其他钩子。</param>
    public void Complete( bool skipRemainingPostfixes = true );

    /// <summary>
    /// 如果您在回调中进行资源/资源包加载调用，则禁用递归调用。
    /// 如果您想防止递归，应该在加载资源/资源包之前调用此方法。
    /// </summary>
    public void DisableRecursion();

    /// <summary>
    /// 获取调用资源加载调用时使用的原始参数。
    /// </summary>
    public AssetLoadedParameters Parameters { get; }

    /// <summary>
    /// 获取与加载资源关联的AssetBundle。
    /// </summary>
    public AssetBundle Bundle { get; }

    /// <summary>
    /// 获取加载的资源。覆盖单个索引以更改将要加载的资源引用。
    ///
    /// 如果加载类型是'LoadByType'或'LoadNamedWithSubAssets'，并且您订阅了'OneCallbackPerLoadCall'，请考虑使用此属性。
    /// </summary>
    public UnityEngine.Object[] Assets { get; set; }

    /// <summary>
    /// 获取加载的资源。这简单地等于Assets属性的第一个索引，并有一些
    /// 额外的null保护以防止在使用时出现NullReferenceExceptions。
    /// </summary>
    public UnityEngine.Object Asset { get; set; }
}
```

HookBehaviour是一个具有以下定义的枚举：

```C#
/// <summary>
/// 指示资源重定向器应如何处理回调的枚举。
/// </summary>
public enum HookBehaviour
{
    /// <summary>
    /// 指定每次调用资源/资源加载方法时应接收恰好一个回调。
    /// </summary>
    OneCallbackPerLoadCall = 1,

    /// <summary>
    /// 指定每个加载的资源/资源应接收恰好一个回调。这意味着
    /// 应该在上下文对象上使用'Asset'属性而不是'Assets'属性。
    /// 请注意，使用此选项时，如果加载调用未返回任何资源，将不会收到任何回调。
    /// </summary>
    OneCallbackPerResourceLoaded = 2
}
```

这里需要注意的重要一点是，上下文对象上既有`Asset`属性也有`Assets`属性。这些可以互换使用，但只有在以下两个条件适用时才会使用数组：
 * 您已订阅了`OneCallbackPerLoadCall`。
 * `Parameters`属性中的`LoadType`是`LoadByType`或`LoadNamedWithSubAssets`，这是唯一可能返回多个资源的资源加载类型。

与此相关，值得一提的是，如果加载资源的调用返回0个资源，如果您通过`OneCallbackPerResourceLoaded`订阅，您将不会收到任何回调，而如果您通过`OneCallbackPerLoadCall`订阅，您仍然会收到一个回调。

如果您更新或替换正在加载的资源，请记住调用`Complete`方法来指示您的意图：
 * 是否应该调用剩余的后缀。

此外，如果我们查看上下文对象的`Parameters`属性，我们会找到以下定义：

```C#
/// <summary>
/// 表示加载调用的原始参数的类。
/// </summary>
public class AssetLoadedParameters
{
    /// <summary>
    /// 获取正在加载的资源的名称。如果通过'LoadMainAsset'或'LoadByType'加载，将为null。
    /// </summary>
    public string Name { get; }

    /// <summary>
    /// 获取传递给资源加载调用的类型。
    /// </summary>
    public Type Type { get; }

    /// <summary>
    /// 获取加载此资源的调用类型。如果指定了'LoadByType'或'LoadNamedWithSubAssets'，
    /// 如果订阅为'OneCallbackPerLoadCall'，可能会返回多个资源。
    /// </summary>
    public AssetLoadType LoadType { get; }
}

/// <summary>
/// 表示资源可能被加载的不同方式的枚举。
/// </summary>
public enum AssetLoadType
{
    /// <summary>
    /// 指示此资源已在AssetBundle API中作为'mainAsset'加载。
    /// </summary>
    LoadMainAsset,

    /// <summary>
    /// 指示此调用正在AssetBundle API中加载特定类型的所有资源。
    /// </summary>
    LoadByType,

    /// <summary>
    /// 指示此调用正在AssetBundle API中加载特定命名的资源。
    /// </summary>
    LoadNamed,

    /// <summary>
    /// 指示此调用正在AssetBundle API中加载特定命名的资源及其下方的所有资源。
    /// </summary>
    LoadNamedWithSubAssets
}
```

还值得一提的是，这些钩子处理资源的同步和异步加载。

通过钩子行为`OneCallbackPerLoadCall`订阅的钩子将在具有行为`OneCallbackPerResourceLoaded`的钩子之前被调用。

### 资源加载方法
资源重定向器在从`Resources` API加载资源时只提供后缀回调。

事件回调链看起来像这样：`[原始方法] => [ResourceLoaded钩子]`。`ResourceLoaded`事件处理同步和异步加载资源的后缀。

#### 资源已加载方法
方法`RegisterResourceLoadedHook( HookBehaviour behaviour, Action<ResourceLoadedContext> action )`钩入UnityEngine中的`Resources` API。每当通过此API加载资源时，都会向这些钩子发送回调。

此API是`Resources` API的后缀钩子，这意味着它在原始资源已经加载之后首次被调用，但仍可替换。

`ResourceLoadedContext`类具有以下定义：

```C#
/// <summary>
/// 围绕ResourceLoaded钩子的操作上下文。
/// </summary>
public class ResourceLoadedContext : IAssetOrResourceLoadedContext
{
    /// <summary>
    /// 获取一个布尔值，指示此资源是否已经被重定向过。
    /// </summary>
    public bool HasReferenceBeenRedirectedBefore( UnityEngine.Object asset );

    /// <summary>
    /// 获取特定资源的文件系统路径，该路径应该是唯一的。
    /// </summary>
    /// <param name="asset"></param>
    /// <returns></returns>
    public string GetUniqueFileSystemAssetPath( UnityEngine.Object asset );

    /// <summary>
    /// 指示您的工作已完成，以及是否应该调用对此资源/资源加载的任何其他钩子。
    /// </summary>
    /// <param name="skipRemainingPostfixes">指示是否应该跳过任何其他钩子。</param>
    public void Complete( bool skipRemainingPostfixes = true );

    /// <summary>
    /// 如果您在回调中进行资源/资源包加载调用，则禁用递归调用。
    /// 如果您想防止递归，应该在加载资源/资源包之前调用此方法。
    /// </summary>
    public void DisableRecursion();

    /// <summary>
    /// 获取调用资源加载调用时使用的原始参数。
    /// </summary>
    public ResourceLoadParameters Parameters { get; }

    /// <summary>
    /// 获取加载的资源。覆盖单个索引以更改将要加载的资源引用。
    ///
    /// 如果加载类型是'LoadByType'，并且您订阅了'OneCallbackPerLoadCall'，请考虑使用此属性。
    /// </summary>
    public UnityEngine.Object[] Assets { get; set; }

    /// <summary>
    /// 获取加载的资源。这简单地等于Assets属性的第一个索引，并有一些
    /// 额外的null保护以防止在使用时出现NullReferenceExceptions。
    /// </summary>
    public UnityEngine.Object Asset { get; set; }
}
```

HookBehaviour是一个具有以下定义的枚举：

```C#
/// <summary>
/// 指示资源重定向器应如何处理回调的枚举。
/// </summary>
public enum HookBehaviour
{
    /// <summary>
    /// 指定每次调用资源/资源加载方法时应接收恰好一个回调。
    /// </summary>
    OneCallbackPerLoadCall = 1,

    /// <summary>
    /// 指定每个加载的资源/资源应接收恰好一个回调。这意味着
    /// 应该在上下文对象上使用'Asset'属性而不是'Assets'属性。
    /// 请注意，使用此选项时，如果加载调用未返回任何资源，将不会收到任何回调。
    /// </summary>
    OneCallbackPerResourceLoaded = 2
}
```

这里需要注意的重要一点是，上下文对象上既有`Asset`属性也有`Assets`属性。这些可以互换使用，但只有在以下两个条件适用时才会使用数组：
 * 您已订阅了`OneCallbackPerLoadCall`。
 * `Parameters`属性中的`LoadType`是`LoadByType`，这是唯一可能返回多个资源的资源加载类型。

与此相关，值得一提的是，如果加载资源的调用返回0个资源，如果您通过`OneCallbackPerResourceLoaded`订阅，您将不会收到任何回调，而如果您通过`OneCallbackPerLoadCall`订阅，您仍然会收到一个回调。

如果您更新或替换正在加载的资源，请记住调用`Complete`方法来指示您的意图：
 * 是否应该调用剩余的后缀。

此外，如果我们查看上下文对象的`Parameters`属性，我们会找到以下定义：

```C#
/// <summary>
/// 表示加载调用的原始参数的类。
/// </summary>
public class ResourceLoadedParameters
{
    /// <summary>
    /// 获取正在加载的资源的名称。如果使用'LoadByType'，将不是完整的资源路径。
    /// </summary>
    public string Path { get; set; }

    /// <summary>
    /// 获取传递给资源加载调用的类型。
    /// </summary>
    public Type Type { get; set; }

    /// <summary>
    /// 获取加载此资源的调用类型。如果指定了'LoadByType'，
    /// 如果订阅为'OneCallbackPerLoadCall'，可能会返回多个资源。
    /// </summary>
    public ResourceLoadType LoadType { get; }
}

/// <summary>
/// 表示资源可能被加载的不同方式的枚举。
/// </summary>
public enum ResourceLoadType
{
    /// <summary>
    /// 指示此调用正在加载Resources API中特定类型的所有资源（在特定路径下）。
    /// </summary>
    LoadByType,

    /// <summary>
    /// 指示此调用正在加载Resources API中的单个命名资源。
    /// </summary>
    LoadNamed,

    /// <summary>
    /// 指示此调用正在加载Resources API中的单个命名内置资源。
    /// </summary>
    LoadNamedBuiltIn
}
```

还值得一提的是，这些钩子处理资源的同步和异步加载。

通过钩子行为`OneCallbackPerLoadCall`订阅的钩子将在具有行为`OneCallbackPerResourceLoaded`的钩子之前被调用。

### AssetBundle加载方法
还可以钩取`AssetBundles`本身的加载。加载资源包时只支持前缀钩子。

事件回调链看起来像这样：`[AssetBundleLoading/AsyncAssetBundleLoading钩子] => [原始方法]`。

#### AssetBundle同步加载方法
方法`RegisterAssetBundleLoadingHook( Action<AssetBundleLoadingContext> action )`用于钩取同步AssetBundle加载方法。

此API是`AssetBundle` API的前缀，这意味着它在加载AssetBundle之前被调用。

`AssetBundleLoadingContext`类具有以下定义：

```C#
/// <summary>
/// 围绕AssetBundleLoading钩子的操作上下文（同步）。
/// </summary>
public class AssetBundleLoadingContext : IAssetBundleLoadingContext
{
    /// <summary>
    /// 获取资源包的规范化路径，该路径：
    ///  * 相对于当前目录
    ///  * 小写
    ///  * 使用'\'作为分隔符。
    /// </summary>
    /// <returns></returns>
    public string GetNormalizedPath();

    /// <summary>
    /// 指示您的工作已完成，以及是否应该调用对此资源包加载的任何其他钩子。
    /// </summary>
    /// <param name="skipRemainingPrefixes">指示是否应该跳过剩余的前缀。</param>
    /// <param name="skipOriginalCall">指示是否应该跳过原始调用。如果您设置了资源包，您可能希望将其设置为true。</param>
    public void Complete( bool skipRemainingPrefixes = true, bool? skipOriginalCall = true );

    /// <summary>
    /// 如果您在回调中进行资源/资源包加载调用，则禁用递归调用。
    /// 如果您想防止递归，应该在加载资源/资源包之前调用此方法。
    /// </summary>
    public void DisableRecursion();

    /// <summary>
    /// 获取调用的参数。
    /// </summary>
    public AssetBundleLoadingParameters Parameters { get; }

    /// <summary>
    /// 获取或设置正在加载的AssetBundle。
    /// </summary>
    public AssetBundle Bundle { get; set; }
}
```

因为这是一个前缀API，当调用方法时`Bundle`属性将为null，如果您可以处理指定的路径，则需要将其设置为不同的值。

如果您更新`Bundle`属性，请记住调用`Complete`来指示您的意图：
 * 是否应该跳过剩余的前缀。
 * 是否应该跳过原始方法。

此外，如果我们查看上下文对象的`Parameters`属性，我们会找到以下定义：

```C#
/// <summary>
/// 表示加载调用的原始参数的类。
/// </summary>
public class AssetBundleLoadingParameters
{
    /// <summary>
    /// 获取或设置加载的路径。仅对'LoadFromFile'相关。
    /// </summary>
    public string Path { get; }

    /// <summary>
    /// 获取或设置crc。仅对'LoadFromFile'相关。
    /// </summary>
    public uint Crc { get; }

    /// <summary>
    /// 获取或设置偏移量。仅对'LoadFromFile'相关。
    /// </summary>
    public ulong Offset { get; }

    /// <summary>
    /// 获取加载此资源包的调用类型。
    /// </summary>
    public AssetBundleLoadType LoadType { get; }
}

/// <summary>
/// 表示资源包可能被加载的不同方式的枚举。
/// </summary>
public enum AssetBundleLoadType
{
    /// <summary>
    /// 指示资源包正在通过调用'LoadFromFile'或'LoadFromFileAsync'加载。
    /// </summary>
    LoadFromFile,
}
```

如您所见，当前实现仅钩取LoadFromFile/LoadFromFileAsync方式的AssetBundle加载，但这可能会在未来扩展。

还值得查看`GetNormalizedPath()`方法，而不是原始调用参数的`Path`属性。这是因为传递给方法的路径可以采用任何形式：
 * 绝对路径
 * 相对路径
 * 在路径中间包含无关的'..'
 
更改资源包加载操作结果的另一种方法是更改`Parameters`属性中的`Path`、`Crc`和`Offset`属性的值。如果您这样做，您可能不希望调用Complete方法，因为您希望原始方法仍然被调用。

#### AssetBundle异步加载方法
方法`RegisterAsyncAssetBundleLoadingHook( Action<AsyncAssetBundleLoadingContext> action )`用于钩取异步AssetBundle加载方法。

此API是`AssetBundle` API的前缀，这意味着它在创建`AssetBundleCreateRequest`之前被调用。

`AsyncAssetBundleLoadingContext`类具有以下定义：

```C#
/// <summary>
/// 围绕AsyncAssetBundleLoading钩子的操作上下文（异步）。
/// </summary>
public class AsyncAssetBundleLoadingContext : IAssetBundleLoadingContext
{
    /// <summary>
    /// 获取资源包的规范化路径，该路径：
    ///  * 相对于当前目录
    ///  * 小写
    ///  * 使用'\'作为分隔符。
    /// </summary>
    /// <returns></returns>
    public string GetNormalizedPath();

    /// <summary>
    /// 指示您的工作已完成，以及是否应该调用对此资源包加载的任何其他钩子。
    /// </summary>
    /// <param name="skipRemainingPrefixes">指示是否应该跳过剩余的前缀。</param>
    /// <param name="skipOriginalCall">指示是否应该跳过原始调用。如果您设置了请求，您可能希望将其设置为true。</param>
    public void Complete( bool skipRemainingPrefixes = true, bool? skipOriginalCall = true );

    /// <summary>
    /// 如果您在回调中进行资源/资源包加载调用，则禁用递归调用。
    /// 如果您想防止递归，应该在加载资源/资源包之前调用此方法。
    /// </summary>
    public void DisableRecursion();

    /// <summary>
    /// 获取调用的参数。
    /// </summary>
    public AssetBundleLoadingParameters Parameters { get; }

    /// <summary>
    /// 获取或设置用于加载AssetBundle的AssetBundleCreateRequest。
    /// </summary>
    public AssetBundleCreateRequest Request { get; set; }

    /// <summary>
    /// 获取或设置正在加载的AssetBundle。
    /// </summary>
    public AssetBundle Bundle { get; set; }

    /// <summary>
    /// 获取或设置此加载操作应如何解析。
    /// 设置Bundle/Request属性将自动更新此值。
    /// </summary>
    public AsyncAssetBundleLoadingResolve ResolveType { get; set; }
}
```

因为这是一个前缀API，当调用方法时`Request`属性将为null，如果您可以处理指定的路径，则需要将其设置为不同的值。

如您所见，上下文对象上实际上还有一个`Bundle`属性可用。但是，在正常情况下，您不能在`AsyncAssetBundleLoadingContext`上使用`Bundle`属性。为了能够使用这些，您必须首先在初始化逻辑中调用一次`ResourceRedirection.EnableSyncOverAsyncAssetLoads`。这将允许您直接设置bundle，因此您不必通过标准的`AssetBundle` API来获取请求对象。

但是，建议如果可以的话，您应该设置`Request`属性而不是`Bundle`属性，因为这将保持操作异步，并且在加载资源时不会阻塞游戏。

如果您更新`Request`属性，请记住调用`Complete`来指示您的意图：
 * 是否应该跳过剩余的前缀。
 * 是否应该跳过原始方法。

此外，如果我们查看上下文对象的`Parameters`属性，我们会找到以下定义：

```C#
/// <summary>
/// 表示加载调用的原始参数的类。
/// </summary>
public class AssetBundleLoadingParameters
{
    /// <summary>
    /// 获取加载的路径。仅对'LoadFromFile'相关。
    /// </summary>
    public string Path { get; }

    /// <summary>
    /// 获取crc。仅对'LoadFromFile'相关。
    /// </summary>
    public uint Crc { get; }

    /// <summary>
    /// 获取偏移量。仅对'LoadFromFile'相关。
    /// </summary>
    public ulong Offset { get; }

    /// <summary>
    /// 获取加载此资源包的调用类型。
    /// </summary>
    public AssetBundleLoadType LoadType { get; }
}

/// <summary>
/// 表示资源包可能被加载的不同方式的枚举。
/// </summary>
public enum AssetBundleLoadType
{
    /// <summary>
    /// 指示资源包正在通过调用'LoadFromFile'或'LoadFromFileAsync'加载。
    /// </summary>
    LoadFromFile,
}
```

如您所见，当前实现仅钩取LoadFromFile/LoadFromFileAsync方式的AssetBundle加载，但这可能会在未来扩展。

还值得查看`GetNormalizedPath()`方法，而不是原始调用参数的`Path`属性。这是因为传递给方法的路径可以采用任何形式：
 * 绝对路径
 * 相对路径
 * 在路径中间包含无关的'..'
 
更改资源包加载操作结果的另一种方法是更改`Parameters`属性中的`Path`、`Crc`和`Offset`属性的值。如果您这样做，您可能不希望调用Complete方法，因为您希望原始方法仍然被调用。

订阅前缀资源包加载操作的另一个重要方法是通过方法`RegisterAsyncAndSyncAssetBundleLoadingHook( int priority, Action<IAssetBundleLoadingContext> action )`。此方法将处理异步和同步资源包加载操作。`IAssetLoadingContext`是由`AssetBundleLoadingContext`和`AsyncAssetBundleLoadingContext`实现的接口。

请注意，如果您想使用此方法，您必须首先调用方法`EnableSyncOverAsyncAssetLoads()`来启用此功能所需的钩子。

### 关于递归
如您可能已经注意到的，在前面部分显示的所有上下文类都有一个名为`DisableRecursion`的方法，并且在`ResourceRedirection`类上直接有一个名为`DisableRecursionPermanently`的方法。

这些方法的目的，正如其名称所表明的，是禁用递归。这只留下一个问题：什么时候发生递归？

每当您尝试在回调中使用`AssetBundle`或`Resources` API从内部加载资源/资源/资源包时，都会发生递归。本质上，这意味着所有回调（除了加载资源的回调）都将有机会修改由您的回调加载的资源。

这可能并不总是可取的，因此如果您在加载资源之前调用`DisableRecursion`方法，这种递归行为将被禁用。在许多其他情况下，这种行为是非常可取的，因为它意味着设置*正确*优先级的重要性较低，无论这可能是什么。

递归对其他前缀/后缀回调有一个重要的副作用，即如果您在回调中进行递归加载调用，它们将始终被调用，即使您通过`Complete`方法指示它们不应该被调用。因此，如果在您的场景中，避免这种情况很重要，您必须禁用递归。

`DisableRecursionPermanently`为ResourceRedirection API的所有订阅者永久禁用递归，无论谁调用它。这是一个游戏范围的设置，应该由插件开发者之间决定，而不是由个人决定。

这些选项存在只是因为目前尚不知道启用递归是否为插件开发者提供最佳体验。

### 实现资源重定向器
以下是如何实现资源重定向来钩取通过`AssetBundle` API加载的所有`Texture2D`对象的示例：

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
        if( context.Asset is Texture2D texture2d ) // 也作为null检查
        {
            // TODO: 修改、替换或转储纹理
                
            context.Asset = texture2d; // 只有在您创建了新纹理时才需要更新引用
            context.Complete(
                skipRemainingPostfixes: true );
        }
    }
}
```

### 实现AssetBundle重定向器
以下是如何实现资源重定向来将不存在的资源重定向到单独的'mods'目录的示例。

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
            // 游戏正试图加载一个不存在的路径，让我们重定向到我们自己的资源
		    
            // 获取不同的资源路径
            var normalizedPath = context.GetNormalizedPath();
            var modFolderPath = Path.Combine( "mods", normalizedPath );
		    
            // 如果路径存在，让我们加载那个
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
            // 游戏正试图加载一个不存在的路径，让我们重定向到我们自己的资源
		    
            // 获取不同的资源路径
            var normalizedPath = context.GetNormalizedPath();
            var modFolderPath = Path.Combine( "mods", normalizedPath );
		    
            // 如果路径存在，让我们加载那个
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

以下是同时实现同一功能的聪明方法，通过一个同时钩取同步和异步方法的方法：

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
            // 游戏正试图加载一个不存在的路径，让我们重定向到我们自己的资源
	    
            // 获取不同的资源路径
            var normalizedPath = context.GetNormalizedPath();
            var modFolderPath = Path.Combine( "mods", normalizedPath );
	    
            // 如果路径存在，让我们加载那个
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

虽然这很简洁，但会导致所有资源包都被同步加载，可能导致游戏锁定并造成FPS延迟。另一种同时处理这个问题的方法可能是这样的：

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
            // 游戏正试图加载一个不存在的路径，让我们重定向到我们自己的资源
	    
            // 获取不同的资源路径
            var normalizedPath = context.GetNormalizedPath();
            var modFolderPath = Path.Combine( "mods", normalizedPath );
	    
            // 如果路径存在，让我们加载那个
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

以下是插件中启用`EmulateAssetBundles`选项时激活的重定向器，它允许从与游戏请求资源包不同的位置加载资源包：

```C#
/// <summary>
/// 创建一个资源包钩子，如果存在的话，尝试在仿真目录中加载资源包而不是默认资源包。
/// </summary>
/// <param name="hookPriority">钩子的优先级。</param>
/// <param name="emulationDirectory">要在其中查找资源包的目录。</param>
public static void EnableEmulateAssetBundles( int hookPriority, string emulationDirectory )
{
    RegisterAssetBundleLoadingHook( hookPriority, ctx => HandleAssetBundleEmulation( ctx, SetBundle ) );
    RegisterAsyncAssetBundleLoadingHook( hookPriority, ctx => HandleAssetBundleEmulation( ctx, SetRequest ) );
		    
    // 定义基础回调
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
		    
    // 同步特定代码
    void SetBundle( AssetBundleLoadingContext context, string path )
    {
        context.Bundle = AssetBundle.LoadFromFile( path, context.Parameters.Crc, context.Parameters.Offset );
    }
		    
    // 异步特定代码
    void SetRequest( AsyncAssetBundleLoadingContext context, string path )
    {
        context.Request = AssetBundle.LoadFromFileAsync( path, context.Parameters.Crc, context.Parameters.Offset );
    }
}
```

以下是插件中启用`RedirectMissingAssetBundles`选项时激活的重定向器，它基本上只是在找不到资源包时加载一个空资源包：

```C#
/// <summary>
/// 创建一个资源包钩子，如果正在加载的文件不存在，则将资源包加载重定向到空资源包。
/// </summary>
/// <param name="hookPriority">钩子的优先级。</param>
public static void EnableRedirectMissingAssetBundlesToEmptyAssetBundle( int hookPriority )
{
    RegisterAssetBundleLoadingHook( hookPriority, ctx => HandleMissingBundle( ctx, SetBundle ) );
    RegisterAsyncAssetBundleLoadingHook( hookPriority, ctx => HandleMissingBundle( ctx, SetRequest ) );
	    
    // 定义基础回调
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
	    
    // 同步特定代码
    void SetBundle( AssetBundleLoadingContext context, byte[] assetBundleData )
    {
        var bundle = AssetBundle.LoadFromMemory( assetBundleData );
        context.Bundle = bundle;
    }
	    
    // 异步特定代码
    void SetRequest( AsyncAssetBundleLoadingContext context, byte[] assetBundleData )
    {
        var request = AssetBundle.LoadFromMemoryAsync( assetBundleData );
        context.Request = request;
    }
}
```

### 实现资源/资源处理器（自动翻译器）
本节展示如何实现尊重自动翻译器配置的资源/资源重定向器。

`XUnity.AutoTranslator.Core.Plugin.dll`程序集有一个基类，可用于实现一个为翻译目的而转储资源的插件。

这个类只是简单地钩取从`AssetBundle`和`Resources` API加载资源的后缀。以下是基类的样子：

```C#
/// <summary>
/// 资源重定向处理器的基本实现，负责处理对更新或转储重定向资源感兴趣的资源重定向器的管道。
/// </summary>
/// <typeparam name="TAsset">正在重定向的资源类型。</typeparam>
public abstract class AssetLoadedHandlerBaseV2<TAsset>
    where TAsset : UnityEngine.Object
{
    /// <summary>
    /// 当资源应该被更新或替换时调用的方法。
    /// </summary>
    /// <param name="calculatedModificationPath">这是在CalculateModificationFilePath方法中计算的修改路径。</param>
    /// <param name="asset">要更新或替换的资源。</param>
    /// <param name="context">这是包含资源重定向事件所有相关信息的上下文。</param>
    /// <returns>指示事件是否应该被视为已处理的布尔值。</returns>
    protected abstract bool ReplaceOrUpdateAsset( string calculatedModificationPath, ref TAsset asset, IAssetOrResourceLoadedContext context );

    /// <summary>
    /// 当资源应该被转储时调用的方法。
    /// </summary>
    /// <param name="calculatedModificationPath">这是在CalculateModificationFilePath方法中计算的修改路径。</param>
    /// <param name="asset">要更新或替换的资源。</param>
    /// <param name="context">这是包含资源重定向事件所有相关信息的上下文。</param>
    /// <returns>指示事件是否应该被视为已处理的布尔值。</returns>
    protected abstract bool DumpAsset( string calculatedModificationPath, TAsset asset, IAssetOrResourceLoadedContext context );

    /// <summary>
    /// 当触发新的资源事件以计算资源的唯一路径时调用的方法。
    /// </summary>
    /// <param name="asset">要更新或替换的资源。</param>
    /// <param name="context">这是包含资源重定向事件所有相关信息的上下文。</param>
    /// <returns>唯一表示重定向资源路径的字符串。</returns>
    protected abstract string CalculateModificationFilePath( TAsset asset, IAssetOrResourceLoadedContext context );

    /// <summary>
    /// 要调用以指示是否应该处理资源的方法。
    /// </summary>
    /// <param name="asset">要更新或替换的资源。</param>
    /// <param name="context">这是包含资源重定向事件所有相关信息的上下文。</param>
    /// <returns>指示是否应该处理资源的布尔值。</returns>
    protected abstract bool ShouldHandleAsset( TAsset asset, IAssetOrResourceLoadedContext context );
}
```

自动翻译包括此类的一个默认实现，用于TextAssets。它看起来像这样：

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

请注意，当访问资源文件时，我们不使用标准文件API来获取流以获取文件中的数据。相反，我们使用RedirectedDirectory外观。这也会查找ZIP文件，并在进行查找时简单地将ZIP文件视为目录。

此类实现的另一个示例是为恋活（Koikatsu）启用替换其自定义资源：

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

请注意，此实现使用`SimpleTextTranslationCache`来查找翻译。使用此类进行翻译查找具有以下好处：
 * 空白字符不必完全匹配。
 * 它尊重`RedirectedResourceDetectionStrategy`配置。如果不尊重这一点，插件可能会对某些文本进行双重翻译。
 * 加载文本翻译文件时，它支持插件使用的相同文本格式。

一旦实现了其中一个类，您只需要实例化它，它就会发挥其魔力。 
