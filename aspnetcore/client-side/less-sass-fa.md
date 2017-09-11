---
title: "以下であるため、Sass、および ASP.NET Core の優れたフォント"
author: ardalis
description: "Sass、およびフォント優れた小さい ASP.NET Core アプリケーションで使用する方法を説明します。"
keywords: "以下の場合、Sass、フォント優れた、プリプロセッサの ASP.NET Core"
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 94c988f9-95fd-425d-b37e-7f846598c6d4
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/less-sass-fa
ms.openlocfilehash: 4282e992ea80420181f9d7a2e3d6a55d6614c4de
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-styling-applications-with-less-sass-and-font-awesome-in-aspnet-core"></a><span data-ttu-id="d8b75-104">スタイルのアプリケーションより少ないリソースで、Sass、およびフォント優れた ASP.NET Core での概要</span><span class="sxs-lookup"><span data-stu-id="d8b75-104">Introduction to styling applications with Less, Sass, and Font Awesome in ASP.NET Core</span></span>

<span data-ttu-id="d8b75-105">によって[Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="d8b75-105">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="d8b75-106">Web アプリケーションのユーザーには、スタイルを設定し、全体的なエクスペリエンスがますます高期待があります。</span><span class="sxs-lookup"><span data-stu-id="d8b75-106">Users of web applications have increasingly high expectations when it comes to style and overall experience.</span></span> <span data-ttu-id="d8b75-107">最新の web アプリケーションは、豊富なツール、フレームワークの定義および一貫した方法で、ルック アンド フィールを管理するために頻繁に活用します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-107">Modern web applications frequently leverage rich tools and frameworks for defining and managing their look and feel in a consistent manner.</span></span> <span data-ttu-id="d8b75-108">ようなフレームワーク[ブートス トラップ](http://getbootstrap.com/)共通スタイルと web サイトのレイアウト オプションのセットを定義するために効果的に進むことができます。</span><span class="sxs-lookup"><span data-stu-id="d8b75-108">Frameworks like [Bootstrap](http://getbootstrap.com/) can go a long way toward defining a common set of styles and layout options for web sites.</span></span> <span data-ttu-id="d8b75-109">ただし、ほとんどの重要なサイトも恩恵を効果的に定義およびスタイルおよびカスケード スタイル シート (CSS) ファイルでは、管理を行うことだけでなく、サイトのインターフェイスのより直観的なを支援するアイコンの画像以外に簡単にアクセスできること。</span><span class="sxs-lookup"><span data-stu-id="d8b75-109">However, most non-trivial sites also benefit from being able to effectively define and maintain styles and cascading style sheet (CSS) files, as well as having easy access to non-image icons that help make the site's interface more intuitive.</span></span> <span data-ttu-id="d8b75-110">ような場合は言語とツールをサポートする[小さい](http://lesscss.org/)と[Sass](http://sass-lang.com/)などのライブラリおよび[フォント優れた](http://fortawesome.github.io/Font-Awesome/)付属します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-110">That's where languages and tools that support [Less](http://lesscss.org/) and [Sass](http://sass-lang.com/), and libraries like [Font Awesome](http://fortawesome.github.io/Font-Awesome/), come in.</span></span>

## <a name="css-preprocessor-languages"></a><span data-ttu-id="d8b75-111">CSS プリプロセッサ言語</span><span class="sxs-lookup"><span data-stu-id="d8b75-111">CSS preprocessor languages</span></span>

<span data-ttu-id="d8b75-112">基になる言語の操作のエクスペリエンスを改善するために、他の言語にコンパイルされる言語は、"プリプロセッサ"と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="d8b75-112">Languages that are compiled into other languages, in order to improve the experience of working with the underlying language, are referred to as preprocessors.</span></span> <span data-ttu-id="d8b75-113">CSS の 2 つの一般的なプリプロセッサがある: 小さいと Sass です。</span><span class="sxs-lookup"><span data-stu-id="d8b75-113">There are two popular preprocessors for CSS: Less and Sass.</span></span>  <span data-ttu-id="d8b75-114">これらのプリプロセッサでは、変数と入れ子になったルールは、大規模で複雑なスタイル シートの保守容易性を向上させるためのサポートなど、CSS に機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-114">These preprocessors add features to CSS, such as support for variables and nested rules, which improve the maintainability of large, complex stylesheets.</span></span> <span data-ttu-id="d8b75-115">言語としての CSS は非常に基本的なサポートも、変数と、単純なものが欠落していると、傾向が肥大化し、反復的な CSS ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-115">CSS as a language is very basic, lacking support even for something as simple as variables, and this tends to make CSS files repetitive and bloated.</span></span> <span data-ttu-id="d8b75-116">プリプロセッサを使用して実際のプログラミング言語の機能を追加するのに役立つの重複を減らすのスタイル ルールより効率的な整理を提供できます。</span><span class="sxs-lookup"><span data-stu-id="d8b75-116">Adding real programming language features via preprocessors can help reduce duplication and provide better organization of styling rules.</span></span> <span data-ttu-id="d8b75-117">Visual Studio では、両方の以下の組み込みサポートと Sass、だけでなくこれらの言語を使用する場合、開発エクスペリエンスがさらに向上する拡張機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-117">Visual Studio provides built-in support for both Less and Sass, as well as extensions that can further improve the development experience when working with these languages.</span></span>

<span data-ttu-id="d8b75-118">プリプロセッサが読みやすさとスタイル情報の保守容易性を向上する方法の簡単な例としては、この CSS を考慮してください。</span><span class="sxs-lookup"><span data-stu-id="d8b75-118">As a quick example of how preprocessors can improve readability and maintainability of style information, consider this CSS:</span></span>

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

<span data-ttu-id="d8b75-119">以下を使用して、この書き直すことができます、重複をすべて取り除くを使用して、 *mixin* (という名前は"mix"することができます、1 つのクラスまたは別のルール セットからのプロパティ)。</span><span class="sxs-lookup"><span data-stu-id="d8b75-119">Using Less, this can be rewritten to eliminate all of the duplication, using a *mixin* (so named because it allows you to "mix in" properties from one class or rule-set into another):</span></span>

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a><span data-ttu-id="d8b75-120">小さい</span><span class="sxs-lookup"><span data-stu-id="d8b75-120">Less</span></span>

<span data-ttu-id="d8b75-121">小さい CSS プリプロセッサでは、Node.js を使用して実行されます。</span><span class="sxs-lookup"><span data-stu-id="d8b75-121">The Less CSS preprocessor runs using Node.js.</span></span> <span data-ttu-id="d8b75-122">以下をインストールするには、コマンド プロンプトから Node Package Manager (npm) を使用 (-g 意味「グローバル」)。</span><span class="sxs-lookup"><span data-stu-id="d8b75-122">To install Less, use Node Package Manager (npm) from a command prompt (-g means "global"):</span></span>

```console
npm install -g less
```

<span data-ttu-id="d8b75-123">Visual Studio を使用している場合より少ないリソースをプロジェクトに以下の 1 つまたは複数のファイルを追加し、コンパイル時にそれを処理する Gulp (または Grunt) を構成で始めることができます。</span><span class="sxs-lookup"><span data-stu-id="d8b75-123">If you're using Visual Studio, you can get started with Less by adding one or more Less files to your project, and then configuring Gulp (or Grunt) to process them at compile-time.</span></span> <span data-ttu-id="d8b75-124">追加、*スタイル*フォルダーをプロジェクトにして新しい未満という名前のファイルを追加して*main.less*このフォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="d8b75-124">Add a *Styles* folder to your project, and then add a new Less file named *main.less* to this folder.</span></span>

![Less ファイルを追加します。](less-sass-fa/_static/add-less-file.png)

<span data-ttu-id="d8b75-126">追加されると、フォルダー構造は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="d8b75-126">Once added, your folder structure should look something like this:</span></span>

![フォルダー構造](less-sass-fa/_static/folder-structure.png)

<span data-ttu-id="d8b75-128">今すぐ CSS にコンパイルされ、Gulp で wwwroot フォルダーに展開されているファイルをいくつかの基本的なスタイルを追加できます。</span><span class="sxs-lookup"><span data-stu-id="d8b75-128">Now you can add some basic styling to the file, which will be compiled into CSS and deployed to the wwwroot folder by Gulp.</span></span>

<span data-ttu-id="d8b75-129">変更*main.less*に含める次の内容は、1 つの基本色から単純なカラー パレットを作成します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-129">Modify *main.less* to include the following content, which creates a simple color palette from a single base color.</span></span>

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

<span data-ttu-id="d8b75-130">`@base`その他の@-prefixed項目は、変数です。</span><span class="sxs-lookup"><span data-stu-id="d8b75-130">`@base` and the other @-prefixed items are variables.</span></span> <span data-ttu-id="d8b75-131">それぞれは、色を表します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-131">Each of them represents a color.</span></span> <span data-ttu-id="d8b75-132">除く`@base`、カラー関数を使用して設定されます: 明るく、暗くする、および回転します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-132">Except for `@base`, they are set using color functions: lighten, darken, and spin.</span></span> <span data-ttu-id="d8b75-133">明るくして、画面の非常に期待されるものです操作を行います。スピンは、(周囲の色は、ホイール) 度の数によって、色の色合いを調整します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-133">Lighten and darken do pretty much what you would expect; spin adjusts the hue of a color by a number of degrees (around the color wheel).</span></span> <span data-ttu-id="d8b75-134">小さいプロセッサには使用されていない変数を無視するためにどこかに使用する必要はこれらの変数の動作を示すためには、します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-134">The Less processor is smart enough to ignore variables that aren't used, so to demonstrate how these variables work, we need to use them somewhere.</span></span> <span data-ttu-id="d8b75-135">クラスは、`.baseColor`などが生成された CSS ファイルで変数のそれぞれの計算値をデモンストレーションします。</span><span class="sxs-lookup"><span data-stu-id="d8b75-135">The classes `.baseColor`, etc. will demonstrate the calculated values of each of the variables in the CSS file that is produced.</span></span>

### <a name="getting-started"></a><span data-ttu-id="d8b75-136">作業の開始</span><span class="sxs-lookup"><span data-stu-id="d8b75-136">Getting started</span></span>

<span data-ttu-id="d8b75-137">作成、 **npm 構成ファイル**(*package.json*) のプロジェクト フォルダーを参照するように編集および`gulp`と`gulp-less`:</span><span class="sxs-lookup"><span data-stu-id="d8b75-137">Create an **npm Configuration File** (*package.json*) in your project folder and edit it to reference `gulp` and `gulp-less`:</span></span>

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

<span data-ttu-id="d8b75-138">または Visual Studio で、プロジェクトのフォルダーで、コマンド プロンプトでいずれかの依存関係をインストール**ソリューション エクスプ ローラー** (**の依存関係 > npm > パッケージを復元**)。</span><span class="sxs-lookup"><span data-stu-id="d8b75-138">Install the dependencies either at a command prompt in your project folder, or in Visual Studio **Solution Explorer** (**Dependencies > npm > Restore packages**).</span></span>

```console
npm install
```

![VS パッケージを復元します。](less-sass-fa/_static/restore-packages.png)

<span data-ttu-id="d8b75-140">プロジェクト フォルダーに作成、**構成ファイルの Gulp** (*gulpfile.js*) 自動プロセスを定義します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-140">In the project folder, create a **Gulp Configuration File** (*gulpfile.js*) to define the automated process.</span></span>  <span data-ttu-id="d8b75-141">表す未満、ファイル サービスおよび以下を実行するタスクの上部にある変数を追加します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-141">Add a variable at the top of the file to represent Less, and a task to run Less:</span></span>

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

<span data-ttu-id="d8b75-142">開く、**タスク ランナー エクスプ ローラー** (**ビュー > その他の Windows > タスク ランナー エクスプ ローラー**)。</span><span class="sxs-lookup"><span data-stu-id="d8b75-142">Open the **Task Runner Explorer** (**View > Other Windows > Task Runner Explorer**).</span></span> <span data-ttu-id="d8b75-143">、タスク間では、新しいタスクがという名前を表示する必要があります`less`です。</span><span class="sxs-lookup"><span data-stu-id="d8b75-143">Among the tasks, you should see a new task named `less`.</span></span> <span data-ttu-id="d8b75-144">ウィンドウを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8b75-144">You might have to refresh the window.</span></span>

<span data-ttu-id="d8b75-145">実行、`less`タスク、およびするがここに表示される内容のような出力を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d8b75-145">Run the `less` task, and you see output similar to what is shown here:</span></span>

![タスク ランナーが少なく](less-sass-fa/_static/less-task-runner.png)

<span data-ttu-id="d8b75-147">*Wwwroot/css*フォルダーにはここで、新しいファイルが含まれています*main.css*:</span><span class="sxs-lookup"><span data-stu-id="d8b75-147">The *wwwroot/css* folder now contains a new file, *main.css*:</span></span>

![メインの css を作成](less-sass-fa/_static/main-css-created.png)

<span data-ttu-id="d8b75-149">開いている*main.css*を参照してください、次のようなものとします。</span><span class="sxs-lookup"><span data-stu-id="d8b75-149">Open *main.css* and you see something like the following:</span></span>

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

<span data-ttu-id="d8b75-150">単純な HTML ページを追加、 *wwwroot*フォルダー、および参照*main.css*アクションのカラー パレットを表示します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-150">Add a simple HTML page to the *wwwroot* folder, and reference *main.css* to see the color palette in action.</span></span>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

<span data-ttu-id="d8b75-151">ご覧 180 度ですぐにご利用いただけます`@base`生成に使用される`@background`の色の両カラー ホイールが発生しました`@base`:</span><span class="sxs-lookup"><span data-stu-id="d8b75-151">You can see that the 180 degree spin on `@base` used to produce `@background` resulted in the color wheel opposing color of `@base`:</span></span>

![小さいテストの例](less-sass-fa/_static/less-test-screenshot.png)

<span data-ttu-id="d8b75-153">小さいも入れ子になったメディア クエリと入れ子になったルールは、サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-153">Less also provides support for nested rules, as well as nested media queries.</span></span> <span data-ttu-id="d8b75-154">たとえば、CSS 規則の詳細なメニューが生じるような入れ子になった階層を定義するには、このような。</span><span class="sxs-lookup"><span data-stu-id="d8b75-154">For example, defining nested hierarchies like menus can result in verbose CSS rules like these:</span></span>

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

<span data-ttu-id="d8b75-155">理想的にはすべての関連するスタイル ルールは、一緒に CSS ファイル内では実際に何も規則とおそらくブロック コメントを除く、この規則を適用します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-155">Ideally all of the related style rules will be placed together within the CSS file, but in practice there is nothing enforcing this rule except convention and perhaps block comments.</span></span>

<span data-ttu-id="d8b75-156">以下を使用してこれらの同じ規則を定義する次のような。</span><span class="sxs-lookup"><span data-stu-id="d8b75-156">Defining these same rules using Less looks like this:</span></span>

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

<span data-ttu-id="d8b75-157">この場合は、すべての下位要素のことに注意してください`nav`そのスコープ内に含まれています。</span><span class="sxs-lookup"><span data-stu-id="d8b75-157">Note that in this case, all of the subordinate elements of `nav` are contained within its scope.</span></span> <span data-ttu-id="d8b75-158">親要素の任意の繰り返しが不要になった (`nav`、 `li`、 `a`)、あり (ただし、そのいくつかは、2 番目の例では、同じ行に値を作成する場合の結果)、合計行の数がも削除します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-158">There is no longer any repetition of parent elements (`nav`, `li`, `a`), and the total line count has dropped as well (though some of that is a result of putting values on the same lines in the second example).</span></span> <span data-ttu-id="d8b75-159">非常に役に立ちます、組織的、ここではすべて、明示的に範囲指定されたスコープ内で指定した UI 要素の規則の表示を off に設定、ファイルの残りの部分から中かっこでします。</span><span class="sxs-lookup"><span data-stu-id="d8b75-159">It can be very helpful, organizationally, to see all of the rules for a given UI element within an explicitly bounded scope, in this case set off from the rest of the file by curly braces.</span></span>

<span data-ttu-id="d8b75-160">`&`構文では、以下のセレクターの機能 (&)、現在のセレクターの親を表すです。</span><span class="sxs-lookup"><span data-stu-id="d8b75-160">The `&` syntax is a Less selector feature, with & representing the current selector parent.</span></span> <span data-ttu-id="d8b75-161">内で、{...}</span><span class="sxs-lookup"><span data-stu-id="d8b75-161">So, within the a {...}</span></span> <span data-ttu-id="d8b75-162">ブロック、`&`を表す、`a`タグ、およびこのインターフェイス`&:link`は等価`a:link`です。</span><span class="sxs-lookup"><span data-stu-id="d8b75-162">block, `&` represents an `a` tag, and thus `&:link` is equivalent to `a:link`.</span></span>

<span data-ttu-id="d8b75-163">繰り返しと CSS の複雑さには、メディア クエリ、応答性の高いデザインを作成するのには、非常に便利ですが大きく貢献もできます。</span><span class="sxs-lookup"><span data-stu-id="d8b75-163">Media queries, extremely useful in creating responsive designs, can also contribute heavily to repetition and complexity in CSS.</span></span> <span data-ttu-id="d8b75-164">小さいメディアによりクエリが許可クラス内で入れ子にできるように、すべてのクラス定義が別内で繰り返される必要があるトップレベル`@media`要素。</span><span class="sxs-lookup"><span data-stu-id="d8b75-164">Less allows media queries to be nested within classes, so that the entire class definition doesn't need to be repeated within different top-level `@media` elements.</span></span> <span data-ttu-id="d8b75-165">たとえば、応答性の高いメニューの CSS を次に示します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-165">For example, here is CSS for a responsive menu:</span></span>

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

<span data-ttu-id="d8b75-166">これは、として小さいでより定義することができます。</span><span class="sxs-lookup"><span data-stu-id="d8b75-166">This can be better defined in Less as:</span></span>

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

<span data-ttu-id="d8b75-167">すでに未満の別の機能は、事前に定義された変数から構築されているスタイル属性を許可する数学的演算のサポートです。</span><span class="sxs-lookup"><span data-stu-id="d8b75-167">Another feature of Less that we have already seen is its support for mathematical operations, allowing style attributes to be constructed from pre-defined variables.</span></span> <span data-ttu-id="d8b75-168">これにより、基本変数を修正して、すべての従属変数の値が自動的に変更するために、関連するスタイルはるかに簡単を更新します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-168">This makes updating related styles much easier, since the base variable can be modified and all dependent values change automatically.</span></span>

<span data-ttu-id="d8b75-169">CSS ファイルで、特に大規模なサイトの (およびメディア クエリが使用されている場合は特に)、非常に大きくなる時間の経過と共にそれらの操作を扱いにくくする傾向があります。</span><span class="sxs-lookup"><span data-stu-id="d8b75-169">CSS files, especially for large sites (and especially if media queries are being used), tend to get quite large over time, making working with them unwieldy.</span></span> <span data-ttu-id="d8b75-170">Less ファイルは、個別に一緒に使用し、プル`@import`ディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="d8b75-170">Less files can be defined separately, then pulled together using `@import` directives.</span></span> <span data-ttu-id="d8b75-171">小さいこともできますファイルをインポートする個別 CSS 同様に、必要な場合です。</span><span class="sxs-lookup"><span data-stu-id="d8b75-171">Less can also be used to import individual CSS files, as well, if desired.</span></span>

<span data-ttu-id="d8b75-172">*Mixins*パラメーターを受け入れることができ、mixin ガードは、特定 mixins が有効になる場合を定義する手段に宣言の形式で条件ロジックをサポート小さくします。</span><span class="sxs-lookup"><span data-stu-id="d8b75-172">*Mixins* can accept parameters, and Less supports conditional logic in the form of mixin guards, which provide a declarative way to define when certain mixins take effect.</span></span> <span data-ttu-id="d8b75-173">Mixin guard の一般的な用途は光に基づく色を調整するか、元の色は濃色。</span><span class="sxs-lookup"><span data-stu-id="d8b75-173">A common use for mixin guards is to adjust colors based on how light or dark the source color is.</span></span> <span data-ttu-id="d8b75-174">色のパラメーターを受け入れる mixin を指定するには、mixin ガードは、その色に基づいた mixin の変更を使用できます。</span><span class="sxs-lookup"><span data-stu-id="d8b75-174">Given a mixin that accepts a parameter for color, a mixin guard can be used to modify the mixin based on that color:</span></span>

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

<span data-ttu-id="d8b75-175">指定された、現在`@base`の値`#663333`、次の CSS をこの少ないスクリプトが生成されます。</span><span class="sxs-lookup"><span data-stu-id="d8b75-175">Given our current `@base` value of `#663333`, this Less script will produce the following CSS:</span></span>

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

<span data-ttu-id="d8b75-176">いくつかの追加の機能は、小さいが、ことができるかについてのこの機能のアイデア言語を前処理します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-176">Less provides a number of additional features, but this should give you some idea of the power of this preprocessing language.</span></span>

## <a name="sass"></a><span data-ttu-id="d8b75-177">Sass</span><span class="sxs-lookup"><span data-stu-id="d8b75-177">Sass</span></span>

<span data-ttu-id="d8b75-178">Sass は、構文は若干異なりますが、同じ機能の多くのサポートを提供するより少ないリソースに似ています。</span><span class="sxs-lookup"><span data-stu-id="d8b75-178">Sass is similar to Less, providing support for many of the same features, but with slightly different syntax.</span></span> <span data-ttu-id="d8b75-179">これは、JavaScript ではなく、Ruby を使用して構築しており、別のセットアップ要件です。</span><span class="sxs-lookup"><span data-stu-id="d8b75-179">It is built using Ruby, rather than JavaScript, and so has different setup requirements.</span></span> <span data-ttu-id="d8b75-180">Sass の元の言語では、中かっこまたはセミコロンを使用していないが、代わりに空白とインデントを使用してスコープを定義します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-180">The original Sass language did not use curly braces or semicolons, but instead defined scope using white space and indentation.</span></span> <span data-ttu-id="d8b75-181">Sass のバージョン 3 では、新しい構文が導入された、 **SCSS** ("Sassy CSS") です。</span><span class="sxs-lookup"><span data-stu-id="d8b75-181">In version 3 of Sass, a new syntax was introduced, **SCSS** ("Sassy CSS").</span></span> <span data-ttu-id="d8b75-182">レベルのインデントおよび空白は無視されます、代わりにセミコロンと中かっこを使用してという点で、SCSS は CSS に似ています。</span><span class="sxs-lookup"><span data-stu-id="d8b75-182">SCSS is similar to CSS in that it ignores indentation levels and whitespace, and instead uses semicolons and curly braces.</span></span>

<span data-ttu-id="d8b75-183">通常 Sass をインストールするには、まず (事前にインストールされた Mac)、Ruby をインストールして実行するとします。</span><span class="sxs-lookup"><span data-stu-id="d8b75-183">To install Sass, typically you would first install Ruby (pre-installed on Mac), and then run:</span></span>

```console
gem install sass
```

<span data-ttu-id="d8b75-184">ただし、Visual Studio を実行している場合開始できます Sass とほぼ同じ方法未満の場合と同様です。</span><span class="sxs-lookup"><span data-stu-id="d8b75-184">However, if you're running Visual Studio, you can get started with Sass in much the same way as you would with Less.</span></span> <span data-ttu-id="d8b75-185">開いている*package.json* "gulp sass"パッケージを追加および`devDependencies`:</span><span class="sxs-lookup"><span data-stu-id="d8b75-185">Open *package.json* and add the "gulp-sass" package to `devDependencies`:</span></span>

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

<span data-ttu-id="d8b75-186">次に、変更*gulpfile.js* sass 変数と Sass ファイルをコンパイルし、結果 wwwroot フォルダーに配置するタスクを追加します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-186">Next, modify *gulpfile.js* to add a sass variable and a task to compile your Sass files and place the results in the wwwroot folder:</span></span>

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

<span data-ttu-id="d8b75-187">Sass ファイルを追加するようになりました*main2.scss*を*スタイル*プロジェクトのルートにフォルダー。</span><span class="sxs-lookup"><span data-stu-id="d8b75-187">Now you can add the Sass file *main2.scss* to the *Styles* folder in the root of the project:</span></span>

![scss ファイルに追加します。](less-sass-fa/_static/add-scss-file.png)

<span data-ttu-id="d8b75-189">開いている*main2.scss*以下を追加します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-189">Open *main2.scss* and add the following:</span></span>

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

<span data-ttu-id="d8b75-190">すべてのファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-190">Save all of your files.</span></span> <span data-ttu-id="d8b75-191">今すぐ更新**Task Runner Explorer**、表示、`sass`タスク。</span><span class="sxs-lookup"><span data-stu-id="d8b75-191">Now when you refresh **Task Runner Explorer**, you see a `sass` task.</span></span> <span data-ttu-id="d8b75-192">実行、およびファイルの場所、 *wwwroot/css*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="d8b75-192">Run it, and look in the */wwwroot/css* folder.</span></span> <span data-ttu-id="d8b75-193">ここでは、 *main2.css*これらの内容を持つファイル。</span><span class="sxs-lookup"><span data-stu-id="d8b75-193">There is now a *main2.css* file, with these contents:</span></span>

```css
body {
    background-color: #CC0000;
}
```

<span data-ttu-id="d8b75-194">Sass に似ている小さいが、同様のメリットを提供することが、同じ入れ子がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="d8b75-194">Sass supports nesting in much the same was that Less does, providing similar benefits.</span></span> <span data-ttu-id="d8b75-195">ファイルは、関数を分割してを使用して含まれている、`@import`ディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="d8b75-195">Files can be split up by function and included using the `@import` directive:</span></span>

```sass
@import 'anotherfile';
```

<span data-ttu-id="d8b75-196">Sass サポート mixins 同様を使用して、`@mixin`に定義するキーワードおよび`@include`からこの例のように、それらを含める[sass lang.com](http://sass-lang.com):</span><span class="sxs-lookup"><span data-stu-id="d8b75-196">Sass supports mixins as well, using the `@mixin` keyword to define them and `@include` to include them, as in this example from [sass-lang.com](http://sass-lang.com):</span></span>

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

<span data-ttu-id="d8b75-197">Mixins、に加えて Sass もサポートしています、継承の概念を拡張する別の 1 つのクラスを許可します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-197">In addition to mixins, Sass also supports the concept of inheritance, allowing one class to extend another.</span></span> <span data-ttu-id="d8b75-198">概念的には、混合が少ない CSS コードに似ています。</span><span class="sxs-lookup"><span data-stu-id="d8b75-198">It's conceptually similar to a mixin, but results in less CSS code.</span></span> <span data-ttu-id="d8b75-199">使用される、`@extend`キーワード。</span><span class="sxs-lookup"><span data-stu-id="d8b75-199">It's accomplished using the `@extend` keyword.</span></span> <span data-ttu-id="d8b75-200">Mixins を試すには、次を追加、 *main2.scss*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d8b75-200">To try out mixins, add the following to your *main2.scss* file:</span></span>

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

<span data-ttu-id="d8b75-201">出力を確認*main2.css*実行した後、`sass`内のタスク**Task Runner Explorer**:</span><span class="sxs-lookup"><span data-stu-id="d8b75-201">Examine the output in *main2.css* after running the `sass` task in **Task Runner Explorer**:</span></span>

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

<span data-ttu-id="d8b75-202">すべてのアラート mixin の一般的なプロパティが各クラスで繰り返されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="d8b75-202">Notice that all of the common properties of the alert mixin are repeated in each class.</span></span> <span data-ttu-id="d8b75-203">Mixin でした、適切なジョブが、開発時に重複を排除することができ、多数の重複していること、必要な CSS ファイルで、潜在的なパフォーマンスの問題よりも大きくなります結果として得られるで CSS をまだ作成しています。</span><span class="sxs-lookup"><span data-stu-id="d8b75-203">The mixin did a good job of helping eliminate duplication at development time, but it's still creating CSS with a lot of duplication in it, resulting in larger than necessary CSS files - a potential performance issue.</span></span>

<span data-ttu-id="d8b75-204">アラート mixin に置き換える、`.alert`クラス、および変更`@include`に`@extend`(拡張を記述する`.alert`ではなく、 `alert`)。</span><span class="sxs-lookup"><span data-stu-id="d8b75-204">Now replace the alert mixin with a `.alert` class, and change `@include` to `@extend` (remembering to extend `.alert`, not `alert`):</span></span>

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

<span data-ttu-id="d8b75-205">Sass をもう一度実行し、結果として得られる CSS を確認します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-205">Run Sass once more, and examine the resulting CSS:</span></span>

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

<span data-ttu-id="d8b75-206">プロパティが、必要に応じて何度もとしてのみ定義されているようになりましたし、CSS が生成される向上します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-206">Now the properties are defined only as many times as needed, and better CSS is generated.</span></span>

<span data-ttu-id="d8b75-207">Sass には、関数と、以下のような条件付きロジック操作も含まれています。</span><span class="sxs-lookup"><span data-stu-id="d8b75-207">Sass also includes functions and conditional logic operations, similar to Less.</span></span> <span data-ttu-id="d8b75-208">実際には、2 つの言語の機能は、非常に似ています。</span><span class="sxs-lookup"><span data-stu-id="d8b75-208">In fact, the two languages' capabilities are very similar.</span></span>

## <a name="less-or-sass"></a><span data-ttu-id="d8b75-209">小さいまたは Sass しますか?</span><span class="sxs-lookup"><span data-stu-id="d8b75-209">Less or Sass?</span></span>

<span data-ttu-id="d8b75-210">まだや Sass の使用が少なく、通常方が優れているかどうかについての合意がない (または元の Sass または Sass 内で新しい SCSS 構文を使用するかどうかも)。</span><span class="sxs-lookup"><span data-stu-id="d8b75-210">There is still no consensus as to whether it's generally better to use Less or Sass (or even whether to prefer the original Sass or the newer SCSS syntax within Sass).</span></span> <span data-ttu-id="d8b75-211">最も重要な意思決定が、おそらく**これらのツールのいずれかを使用して**だけ手動コーディングの CSS ファイルではなく、します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-211">Probably the most important decision is to **use one of these tools**, as opposed to just hand-coding your CSS files.</span></span> <span data-ttu-id="d8b75-212">作成した後も、両方の以下の意思決定し、Sass は適切な選択肢です。</span><span class="sxs-lookup"><span data-stu-id="d8b75-212">Once you've made that decision, both Less and Sass are good choices.</span></span>

## <a name="font-awesome"></a><span data-ttu-id="d8b75-213">優れたフォント</span><span class="sxs-lookup"><span data-stu-id="d8b75-213">Font Awesome</span></span>

<span data-ttu-id="d8b75-214">CSS プリプロセッサに加えて最新の web アプリケーションのスタイル処理の重要なリソースは、フォントすばらしいです。</span><span class="sxs-lookup"><span data-stu-id="d8b75-214">In addition to CSS preprocessors, another great resource for styling modern web applications is Font Awesome.</span></span> <span data-ttu-id="d8b75-215">フォント Awesome は、web アプリケーションで自由に使用できる 500 を超えるスケーラブル ベクター アイコンを提供するツールキットです。</span><span class="sxs-lookup"><span data-stu-id="d8b75-215">Font Awesome is a toolkit that provides over 500 scalable vector icons that can be freely used in your web applications.</span></span> <span data-ttu-id="d8b75-216">ブートス トラップを使用すること、もともとられていますが依存関係またはそのフレームワークに JavaScript ライブラリにします。</span><span class="sxs-lookup"><span data-stu-id="d8b75-216">It was originally designed to work with Bootstrap, but it has no dependency on that framework or on any JavaScript libraries.</span></span>

<span data-ttu-id="d8b75-217">フォント サービスを開始する最も簡単な方法では、パブリックのコンテンツ配信ネットワーク (CDN) の場所を使用してへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-217">The easiest way to get started with Font Awesome is to add a reference to it, using its public content delivery network (CDN) location:</span></span>

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

<span data-ttu-id="d8b75-218">追加することも、Visual Studio プロジェクトに「依存関係」に追加することによってで*bower.json*:</span><span class="sxs-lookup"><span data-stu-id="d8b75-218">You can also add it to your Visual Studio project by adding it to the "dependencies" in *bower.json*:</span></span>

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

<span data-ttu-id="d8b75-219">フォントの優れたへの参照をページにある場合は後に、追加できますアイコン アプリケーション フォント優れたクラス、通常の付いた「fa-」で、インライン HTML 要素に適用することで (など`<span>`または`<i>`)。</span><span class="sxs-lookup"><span data-stu-id="d8b75-219">Once you have a reference to Font Awesome on a page, you can add icons to your application by applying Font Awesome classes, typically prefixed with "fa-", to your inline HTML elements (such as `<span>` or `<i>`).</span></span>  <span data-ttu-id="d8b75-220">たとえば、単純なリストおよび次のようにコードを使用してメニューにアイコンを追加できます。</span><span class="sxs-lookup"><span data-stu-id="d8b75-220">For example, you can add icons to simple lists and menus using code like this:</span></span>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

<span data-ttu-id="d8b75-221">これにより、ブラウザーでは、次が生成されます。-各項目の横にあるアイコンに注意してください。</span><span class="sxs-lookup"><span data-stu-id="d8b75-221">This produces the following in the browser - note the icon beside each item:</span></span>

![リストのアイコン](less-sass-fa/_static/list-icons-screenshot.png)

<span data-ttu-id="d8b75-223">ここで可能なアイコンの完全な一覧を表示することができます。</span><span class="sxs-lookup"><span data-stu-id="d8b75-223">You can view a complete list of the available icons here:</span></span>

<span data-ttu-id="d8b75-224">http://fontawesome.io/icons/</span><span class="sxs-lookup"><span data-stu-id="d8b75-224">http://fontawesome.io/icons/</span></span>

## <a name="summary"></a><span data-ttu-id="d8b75-225">概要</span><span class="sxs-lookup"><span data-stu-id="d8b75-225">Summary</span></span>

<span data-ttu-id="d8b75-226">最新の web アプリケーションは、ますますは、クリーン、直感的で、およびさまざまなデバイスから簡単に使用される応答性の高い、滑らかな設計を要求します。</span><span class="sxs-lookup"><span data-stu-id="d8b75-226">Modern web applications increasingly demand responsive, fluid designs that are clean, intuitive, and easy to use from a variety of devices.</span></span> <span data-ttu-id="d8b75-227">これらの目標を達成するために必要な CSS スタイル シートの複雑さを管理すると、小さいプリプロセッサ like を使用してまたは Sass 最適な方法はします。</span><span class="sxs-lookup"><span data-stu-id="d8b75-227">Managing the complexity of the CSS stylesheets required to achieve these goals is best done using a preprocessor like Less or Sass.</span></span> <span data-ttu-id="d8b75-228">さらに、ツールキット フォント素晴らしいと同様には、テキストのナビゲーション メニューによく知られているアイコンを迅速に提供し、ボタン、全体的なユーザーの向上が、アプリケーションのエクスペリエンスします。</span><span class="sxs-lookup"><span data-stu-id="d8b75-228">In addition, toolkits like Font Awesome quickly provide well-known icons to textual navigation menus and buttons, improving the overall user experience of your application.</span></span>