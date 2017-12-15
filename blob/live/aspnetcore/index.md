---
title: "ASP.NET Core の概要"
author: rick-anderson
description: "ASP.NET Core の概要を提供する。"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 09/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: a075c63fddb9e28a1da37d4ef6647808a0dcb583
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="9ba30-104">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="9ba30-104">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="9ba30-105">著者: [Daniel Roth](https://github.com/danroth27)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="9ba30-105">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="9ba30-106">ASP.NET Core は、インターネットに接続された最新のクラウド ベース アプリケーションを構築するための、クロス プラットフォームで高パフォーマンスの[オープン ソース](https://github.com/aspnet/home) フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="9ba30-106">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="9ba30-107">ASP.NET Core では次のことができます。</span><span class="sxs-lookup"><span data-stu-id="9ba30-107">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="9ba30-108">Web アプリ、Web サービス、[IoT](https://www.microsoft.com/en-us/internet-of-things/) アプリ、モバイル バックエンドを構築する。</span><span class="sxs-lookup"><span data-stu-id="9ba30-108">Build web apps and services, [IoT](https://www.microsoft.com/en-us/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="9ba30-109">Windows、macOS、Linux で好みの開発ツールを使う。</span><span class="sxs-lookup"><span data-stu-id="9ba30-109">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="9ba30-110">クラウドまたはオンプレミスに展開する。</span><span class="sxs-lookup"><span data-stu-id="9ba30-110">Deploy to the cloud or on-premises</span></span>
* <span data-ttu-id="9ba30-111">[.NET Core または .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) 上で実行する。</span><span class="sxs-lookup"><span data-stu-id="9ba30-111">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="9ba30-112">ASP.NET Core を使う理由</span><span class="sxs-lookup"><span data-stu-id="9ba30-112">Why use ASP.NET Core?</span></span>

<span data-ttu-id="9ba30-113">何百万人もの開発者が、これまで、そして現在も、Web アプリの作成に ASP.NET を使っています。</span><span class="sxs-lookup"><span data-stu-id="9ba30-113">Millions of developers have used (and continue to use) ASP.NET to create web apps.</span></span> <span data-ttu-id="9ba30-114">ASP.NET Core は ASP.NET を設計し直したものであり、無駄のないモジュール形式のフレームワークになるようにアーキテクチャが変更されています。</span><span class="sxs-lookup"><span data-stu-id="9ba30-114">ASP.NET Core is a redesign of ASP.NET, with architectural changes that result in a leaner and modular framework.</span></span>

<span data-ttu-id="9ba30-115">ASP.NET Core の利点は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="9ba30-115">ASP.NET Core provides the following benefits:</span></span>

* <span data-ttu-id="9ba30-116">Web UI と Web API を構築するプロセスの統一。</span><span class="sxs-lookup"><span data-stu-id="9ba30-116">A unified story for building web UI and web APIs.</span></span>
* <span data-ttu-id="9ba30-117">[最新のクライアント側フレームワーク](xref:client-side/index)と開発ワークフローの統合。</span><span class="sxs-lookup"><span data-stu-id="9ba30-117">Integration of [modern client-side frameworks](xref:client-side/index) and development workflows.</span></span>
* <span data-ttu-id="9ba30-118">クラウド対応で環境ベースの[構成システム](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="9ba30-118">A cloud-ready, environment-based [configuration system](xref:fundamentals/configuration/index).</span></span>
* <span data-ttu-id="9ba30-119">組み込まれている[依存性の注入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="9ba30-119">Built-in [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="9ba30-120">軽量で高パフォーマンスのモジュール化された HTTP 要求パイプライン。</span><span class="sxs-lookup"><span data-stu-id="9ba30-120">A lightweight, high-performance, and modular HTTP request pipeline.</span></span>
* <span data-ttu-id="9ba30-121">IIS でホストする、または独自のプロセスで自己ホストする機能。</span><span class="sxs-lookup"><span data-stu-id="9ba30-121">Ability to host on IIS or self-host in your own process.</span></span>
* <span data-ttu-id="9ba30-122">[.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) で実行可能であり、アプリの真のサイド バイ サイド バージョン管理をサポート。</span><span class="sxs-lookup"><span data-stu-id="9ba30-122">Can run on [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), which supports true side-by-side app versioning.</span></span>
* <span data-ttu-id="9ba30-123">最新の Web 開発を簡単にするツール。</span><span class="sxs-lookup"><span data-stu-id="9ba30-123">Tooling that simplifies modern web development.</span></span>
* <span data-ttu-id="9ba30-124">Windows、macOS、Linux でビルドおよび実行する機能。</span><span class="sxs-lookup"><span data-stu-id="9ba30-124">Ability to build and run on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="9ba30-125">オープン ソースでコミュニティ重視。</span><span class="sxs-lookup"><span data-stu-id="9ba30-125">Open-source and community-focused.</span></span>

<span data-ttu-id="9ba30-126">ASP.NET Core は、[NuGet](https://www.nuget.org/) パッケージとして完全に提供されます。</span><span class="sxs-lookup"><span data-stu-id="9ba30-126">ASP.NET Core ships entirely as [NuGet](https://www.nuget.org/) packages.</span></span> <span data-ttu-id="9ba30-127">これにより、必要な NuGet パッケージだけを含むようにアプリを最適化できます。</span><span class="sxs-lookup"><span data-stu-id="9ba30-127">This allows you to optimize your app to include just the NuGet packages you need.</span></span> <span data-ttu-id="9ba30-128">小さいアプリ領域の利点には、セキュリティの強化、サービスの削減、パフォーマンスの向上などがあります。</span><span class="sxs-lookup"><span data-stu-id="9ba30-128">The benefits of a smaller app surface area include tighter security, reduced servicing, and improved performance.</span></span>

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="9ba30-129">ASP.NET Core MVC を使って Web API と Web UI を構築する</span><span class="sxs-lookup"><span data-stu-id="9ba30-129">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="9ba30-130">ASP.NET Core MVC は、[Web API](xref:tutorials/index#building-web-apis) と [Web アプリ](xref:tutorials/index#building-web-applications)の構築を支援する機能を備えています。</span><span class="sxs-lookup"><span data-stu-id="9ba30-130">ASP.NET Core MVC provides features that help you build [web APIs](xref:tutorials/index#building-web-apis) and [web apps](xref:tutorials/index#building-web-applications):</span></span>

* <span data-ttu-id="9ba30-131">[モデル ビュー コントローラー (MVC) パターン](xref:mvc/overview)は、Web API と Web アプリを[テスト可能](testing/index.md)にするのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="9ba30-131">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](testing/index.md).</span></span>
* <span data-ttu-id="9ba30-132">[Razor ページ](xref:mvc/razor-pages/index) (2.0 の新機能) はページ ベースのプログラミング モデルであり、Web UI の開発を容易にし、生産性を高めます。</span><span class="sxs-lookup"><span data-stu-id="9ba30-132">[Razor Pages](xref:mvc/razor-pages/index) (new in 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="9ba30-133">[Razor 構文](xref:mvc/views/razor)では、[Razor ページ](xref:mvc/razor-pages/index)および [MVC ビュー](xref:mvc/views/overview)用に生産性の高い言語が提供されます。</span><span class="sxs-lookup"><span data-stu-id="9ba30-133">[Razor syntax](xref:mvc/views/razor) provides a productive language for [Razor Pages](xref:mvc/razor-pages/index) and [MVC Views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="9ba30-134">[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)を使うと、Razor ファイルでの HTML 要素の作成とレンダリングに、サーバー側コードを組み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="9ba30-134">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="9ba30-135">[複数のデータ形式とコンテンツ ネゴシエーション](mvc/models/formatting.md)の組み込みサポートにより、Web API はブラウザーやモバイル デバイスなどのさまざまなクライアントと接続できます。</span><span class="sxs-lookup"><span data-stu-id="9ba30-135">Built-in support for [multiple data formats and content negotiation](mvc/models/formatting.md) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="9ba30-136">[モデル バインド](xref:mvc/models/model-binding)は、HTTP 要求からアクション メソッドのパラメーターにデータを自動的にマップします。</span><span class="sxs-lookup"><span data-stu-id="9ba30-136">[Model Binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="9ba30-137">[モデル検証](xref:mvc/models/validation)は、クライアント側とサーバー側の検証を自動的に実行します。</span><span class="sxs-lookup"><span data-stu-id="9ba30-137">[Model Validation](xref:mvc/models/validation) automatically performs client and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="9ba30-138">クライアント側の開発</span><span class="sxs-lookup"><span data-stu-id="9ba30-138">Client-side development</span></span>

<span data-ttu-id="9ba30-139">ASP.NET Core は、[AngularJS](xref:client-side/angular)、[KnockoutJS](xref:client-side/knockout)、[Bootstrap](xref:client-side/bootstrap) など、さまざまなクライアント側フレームワークとシームレスに統合するように設計されています。</span><span class="sxs-lookup"><span data-stu-id="9ba30-139">ASP.NET Core is designed to integrate seamlessly with a variety of client-side frameworks, including [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout), and [Bootstrap](xref:client-side/bootstrap).</span></span> <span data-ttu-id="9ba30-140">詳しくは、「[クライアント側の開発](client-side/index.md)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="9ba30-140">See [Client-side development](client-side/index.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ba30-141">次のステップ</span><span class="sxs-lookup"><span data-stu-id="9ba30-141">Next steps</span></span>

<span data-ttu-id="9ba30-142">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="9ba30-142">For more information, see the following resources:</span></span>

* [<span data-ttu-id="9ba30-143">ASP.NET Core チュートリアル</span><span class="sxs-lookup"><span data-stu-id="9ba30-143">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* [<span data-ttu-id="9ba30-144">ASP.NET Core の基礎</span><span class="sxs-lookup"><span data-stu-id="9ba30-144">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="9ba30-145">[週 1 回の ASP.NET Community Standup](https://live.asp.net/) では、チームの進行状況とプランが報告され、新しいブログやサード パーティ製ソフトウェアが採り上げられています。</span><span class="sxs-lookup"><span data-stu-id="9ba30-145">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans and features new blogs and third-party software.</span></span>